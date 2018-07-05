---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Introdução ao banco de dados do Entity Framework 4.0 First e 4 do ASP.NET Web Forms - parte 7 | Microsoft Docs
author: tdykstra
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 14ec7a22e9e00ac793fa5a278243e6406a212903
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401799"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Introdução ao banco de dados do Entity Framework 4.0 First e 4 do Web Forms do ASP.NET – parte 7
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial na série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Usando procedimentos armazenados

No tutorial anterior, você implementou um padrão de herança de tabela por hierarquia. Este tutorial mostra como usar procedimentos armazenados para obter mais controle sobre o acesso de banco de dados.

O Entity Framework permite que você especifique o que ele deve usar procedimentos armazenados para acesso ao banco de dados. Para qualquer tipo de entidade, você pode especificar um procedimento armazenado a ser usado para criação, atualização ou exclusão de entidades desse tipo. Em seguida, no modelo de dados, você pode adicionar referências a procedimentos armazenados que você pode usar para executar tarefas como recuperar conjuntos de entidades.

Usando procedimentos armazenados é um requisito comum para acesso ao banco de dados. Em alguns casos, um administrador de banco de dados pode exigir que todo o acesso de banco de dados passar por procedimentos armazenados por motivos de segurança. Em outros casos, você talvez queira criar lógica de negócios para alguns dos processos que o Entity Framework usa quando ele atualiza o banco de dados. Por exemplo, sempre que uma entidade é excluída, talvez queira copiá-lo para um banco de dados de arquivo morto. Ou sempre que uma linha é atualizada. você deseja gravar uma linha em uma tabela de log que registra que fez a alteração. Você pode executar esses tipos de tarefas em um procedimento armazenado que é chamado sempre que o Entity Framework exclui uma entidade ou atualiza uma entidade.

Como no tutorial anterior, você não criará novas páginas. Em vez disso, você vai alterar a forma como o Entity Framework acessa o banco de dados para algumas das páginas que você já criou.

Neste tutorial, você criará os procedimentos armazenados no banco de dados para a inserção `Student` e `Instructor` entidades. Você adicionará ao modelo de dados e especificará que o Entity Framework deve usá-los para a adição `Student` e `Instructor` entidades no banco de dados. Você também criará um procedimento armazenado que você pode usar para recuperar `Course` entidades.

## <a name="creating-stored-procedures-in-the-database"></a>Criando procedimentos armazenados no banco de dados

(Se você estiver usando o *School.mdf* arquivo de projeto disponível para download com este tutorial, você poderá ignorar esta seção porque os procedimentos armazenados já existem.)

Na **Gerenciador de servidores**, expanda *School.mdf*, clique com botão direito **Stored Procedures**e selecione **adicionar novo procedimento armazenado**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Copie as seguintes instruções SQL e colá-los na janela de procedimento armazenado, substituindo o procedimento armazenado de esqueleto.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` entidades têm quatro propriedades: `PersonID`, `LastName`, `FirstName`, e `EnrollmentDate`. O banco de dados gera o valor da ID automaticamente e o procedimento armazenado aceita parâmetros para as outras três. O procedimento armazenado retorna o valor da chave de registro da nova linha para que o Entity Framework pode manter o controle de que a versão da entidade mantém na memória.

Salve e feche a janela de procedimento armazenado.

Criar um `InsertInstructor` procedimento armazenado da mesma maneira, usando as seguintes instruções SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Crie `Update` procedimentos armazenados para o `Student` e `Instructor` entidades também. (O banco de dados já tem um `DeletePerson` procedimento que funcionará para ambos armazenado `Instructor` e `Student` entidades.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

Neste tutorial, você vai mapear todas as três funções – insert, update e delete – para cada tipo de entidade. O Entity Framework versão 4 permite mapear apenas um ou dois desses funções aos procedimentos armazenados sem mapear os outros, com uma exceção: se você mapear a função de atualização, mas não a função de exclusão, o Entity Framework gerará uma exceção quando você tentativa de excluir uma entidade. No Entity Framework versão 3.5, você não tem essa grande flexibilidade no mapeamento de procedimentos armazenados: se você mapeou a uma função, era necessário mapear todos os três.

Para criar um procedimento armazenado que lê em vez de atualizações de dados, criar um que seleciona todos os `Course` entidades, usando as seguintes instruções SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Adicionar os procedimentos armazenados ao modelo de dados

Agora, os procedimentos armazenados são definidos no banco de dados, mas eles devem ser adicionados ao modelo de dados para disponibilizá-las para o Entity Framework. Abra *Schoolmodel*, clique com botão direito na superfície de design e selecione **atualizar o modelo de banco de dados**. No **Add** guia do **Choose Your Database Objects** caixa de diálogo caixa, expanda **Stored Procedures**, selecione os procedimentos armazenados recém-criado e o `DeletePerson` procedimento armazenado e, em seguida, clique em **concluir**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Os procedimentos armazenados de mapeamento

No designer de modelo de dados, clique com botão direito do `Student` entidade e selecione **mapeamento de procedimento armazenado**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

O **Mapping Details** janela for exibida, na qual você pode especificar procedimentos armazenados que o Entity Framework devem usar para inserir, atualizar e excluir entidades desse tipo.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Defina as **inserir** função **InsertStudent**. A janela mostra uma lista de parâmetros de procedimento armazenado, cada um deles deve ser mapeada para uma propriedade de entidade. Dois desses são mapeadas automaticamente porque os nomes são iguais. Não há nenhuma propriedade de entidade nomeada `FirstName`, portanto, você deve selecionar manualmente `FirstMidName` em uma lista suspensa que mostra as propriedades de entidade disponível. (Isso ocorre porque você alterou o nome da `FirstName` propriedade para `FirstMidName` no primeiro tutorial.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

No mesmo **Mapping Details** janela, o mapa o `Update` funcionar para o `UpdateStudent` procedimento armazenado (Verifique se você especificou `FirstMidName` como o valor de parâmetro para `FirstName`, como você fez o `Insert` procedimento armazenado) e o `Delete` função para o `DeletePerson` procedimento armazenado.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Siga o mesmo procedimento para mapear o insert, update e delete de procedimentos armazenados para instrutores o `Instructor` entidade.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Para procedimentos armazenados que leem em vez de atualização de dados, você deve usar o **Model Browser** janela para mapear o procedimento armazenado para a entidade digitá-lo retorna. No designer de modelo de dados, clique com botão direito a superfície de design e selecione **Model Browser**. Abra o **SchoolModel.Store** nó e abra o **Stored Procedures** nó. Em seguida, clique com botão direito do `GetCourses` procedimento armazenado e selecione **Adicionar importação de função**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

No **Adicionar importação de função** caixa de diálogo **retorna uma coleção de** selecionar **entidades**e, em seguida, selecione `Course` como o tipo de entidade retornada. Quando terminar, clique em **Okey**. Salve e feche o *. edmx* arquivo.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Usando Inserir, atualizar e excluir procedimentos armazenados

Procedimentos armazenados para inserir, atualizar e excluir os dados são usados pelo Entity Framework automaticamente depois de adicioná-las ao modelo de dados e mapeado-los para as entidades apropriadas. Agora você pode executar o *StudentsAdd.aspx* página, e sempre que você cria um novo aluno, o Entity Framework usará o `InsertStudent` procedimento armazenado para adicionar a nova linha para o `Student` tabela.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Execute o *Students.aspx* página e o novo aluno aparece na lista.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Altere o nome para verificar se a função de atualização funciona e, em seguida, exclua o aluno para verificar se a função delete funciona.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Usando procedimentos armazenados Select

O Entity Framework não será executado automaticamente procedimentos armazenados, como `GetCourses`, e você não pode usá-los com o `EntityDataSource` controle. Para usá-las, chamá-los no código.

Abra o *InstructorsCourses.aspx.cs* arquivo. O `PopulateDropDownLists` método usa uma consulta LINQ to Entities para recuperar todas as entidades course para que ele pode executar um loop pela lista e determinar quais de um instrutor é atribuído e quais delas são não atribuídos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Substitua pelo código a seguir:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

A página agora usa o `GetCourses` procedimento armazenado para recuperar a lista de todos os cursos. Execute a página para verificar se ele funciona como antes.

(Propriedades de navegação de entidades recuperadas por um procedimento armazenado não poderá ser populadas automaticamente com os dados relacionados a essas entidades, dependendo do `ObjectContext` configurações padrão. Para obter mais informações, consulte [Carregando objetos relacionados](https://msdn.microsoft.com/library/bb896272.aspx) na biblioteca MSDN.)

No próximo tutorial, você aprenderá a usar a funcionalidade de dados dinâmicos para tornar mais fácil para o programa e teste regras de formatação e validação de dados. Em vez de especificar as regras de cada página da web, como cadeias de caracteres de formato de dados e ou não um campo é obrigatório, você pode especificar essas regras nos metadados do modelo de dados e eles são aplicados automaticamente em cada página.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-8.md)
