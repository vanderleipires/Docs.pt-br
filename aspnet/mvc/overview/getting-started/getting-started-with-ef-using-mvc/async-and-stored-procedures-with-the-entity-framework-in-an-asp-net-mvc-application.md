---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Async e procedimentos armazenados com o Entity Framework em um aplicativo ASP.NET MVC | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 7412b32ac29179dfa319544781d4c7165c58196b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async e procedimentos armazenados com o Entity Framework em um aplicativo ASP.NET MVC
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [baixar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio 2013. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Nos tutoriais anteriores, você aprendeu como ler e atualizar dados usando o modelo de programação síncrono. Neste tutorial, você verá como implementar o modelo de programação assíncrono. Código assíncrono pode ajudar um aplicativo um desempenho melhor porque ele faz melhor uso de recursos do servidor.

Neste tutorial você também verá como usar procedimentos armazenados para inserção, atualização e operações de exclusão em uma entidade.

Por fim, você deverá reimplantar o aplicativo no Azure, juntamente com todas as alterações do banco de dados que você implementou desde a primeira vez que você implantou.

As ilustrações a seguir mostram algumas das páginas que você trabalhará com.

![Página de departamentos](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Criar departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>Por que se preocupar com código assíncrono

Um servidor web tem um número limitado de threads disponíveis e, em situações de alta carga de todos os threads disponíveis podem estar em uso. Quando isso acontece, o servidor não pode processar novas solicitações até que os threads são liberados. Com código síncrono, muitos threads podem ser vinculados ao enquanto eles não são realmente fazendo qualquer trabalho porque está aguardando para e/s concluir. Com um código assíncrono, quando um processo está aguardando e/s ser concluída, o thread é liberado para o servidor a ser usado para processar outras solicitações. Como resultado, o código assíncrono permite que recursos de servidor usar com mais eficiência e o servidor está habilitado para manipular mais tráfego sem atrasos.

Em versões anteriores do .NET, escrever e testar o código assíncrono eram complexo, propenso a erros e difíceis de depurar. No .NET 4.5, gravar, teste e depuração de código assíncrono são muito mais fácil que você geralmente deve gravar código assíncrono, a menos que você tenha um motivo para não. Código assíncrono apresente uma pequena quantidade de sobrecarga, mas para situações de pouco tráfego o impacto no desempenho é insignificante, durante situações de alto tráfego, a melhoria de desempenho potencial é significativa.

Para obter mais informações sobre a programação assíncrona, consulte [o suporte assíncrono do uso .NET 4.5 para evitar o bloqueio de chamadas](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Criar o controlador de departamento

Criar um controlador de departamento da mesma forma que os controladores anteriores, exceto que desta vez selecione o **usar async controlador** caixa de seleção de ações.

![Scaffold de controlador do departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Os seguintes destaques mostram o que foi adicionado para o código síncrono para o `Index` método para torná-la assíncrona:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Quatro alterações foram aplicadas para habilitar a consulta de banco de dados do Entity Framework executar de forma assíncrona:

- O método está marcado com o `async` palavra-chave, que informa ao compilador para gerar retornos de chamada para partes do corpo do método e criar automaticamente o `Task<ActionResult>` objeto que é retornado.
- O tipo de retorno foi alterado de `ActionResult` para `Task<ActionResult>`. O `Task<T>` tipo representa um trabalho contínuo com um resultado do tipo `T`.
- O `await` palavra-chave foi aplicada a chamada de serviço web. Quando o compilador vê esta palavra-chave, nos bastidores ele divide o método em duas partes. A primeira parte termina com a operação que é iniciada de forma assíncrona. A segunda parte é colocada em um método de retorno de chamada que é chamado quando a operação for concluída.
- A versão assíncrona do `ToList` método de extensão foi chamado.

Por que é o `departments.ToList` instrução modificados mas não o `departments = db.Departments` instrução? O motivo é que apenas as instruções que causam consultas ou comandos sejam enviados para o banco de dados seja executadas assincronamente. O `departments = db.Departments` instrução define uma consulta, mas a consulta não é executada até que o `ToList` método é chamado. Portanto, somente o `ToList` método é executado de forma assíncrona.

No `Details` método e o `HttpGet` `Edit` e `Delete` métodos, o `Find` método é o que faz com que uma consulta a ser enviado ao banco de dados, para que seja o método que é executado de forma assíncrona:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

No `Create`, `HttpPost Edit`, e `DeleteConfirmed` métodos, é o `SaveChanges` chamada de método faz com que um comando a ser executado, não instruções como `db.Departments.Add(department)` que causam apenas entidades na memória a ser modificado.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Abra *Views\Department\Index.cshtml*e substitua o código de modelo com o código a seguir:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Esse código altera o título do índice para departamentos, move o nome do administrador para a direita e fornece o nome completo do administrador.

Em criar, excluir, detalhes e editar modos de exibição, altere a legenda para o `InstructorID` campo para "Administrator" da mesma maneira que você alterou o campo de nome de departamento para "Departamento" nas exibições do curso.

Na criar e editar modos de exibição usam o código a seguir:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

Nas exibições de detalhes e Delete, use o seguinte código:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Executar o aplicativo e, em seguida, clique no **departamentos** guia.

![Página de departamentos](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Tudo funciona da mesma forma como os outros controladores, mas neste controlador todas as consultas SQL são execução assíncrona.

Algumas coisas a serem consideradas quando você estiver usando a programação assíncrona com o Entity Framework:

- O código assíncrono não é thread-safe. Em outras palavras, em outras palavras, não tente executar várias operações em paralelo usando a mesma instância de contexto.
- Se você quiser tirar proveito dos benefícios de desempenho do código assíncrono, certifique-se de que qualquer biblioteca de pacotes que você está usando (por exemplo, para paginação), use também assíncrona se ele chamam qualquer método de Entity Framework que causam consultas sejam enviadas para o banco de dados.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Use os procedimentos armazenados para inserir, atualizar e excluir

Alguns desenvolvedores e DBAs preferem usar os procedimentos armazenados para acesso ao banco de dados. Em versões anteriores do Entity Framework, você pode recuperar dados usando um procedimento armazenado por [executando uma consulta SQL bruta](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), mas não é possível instruir o EF usar procedimentos armazenados para operações de atualização. Em 6 de EF é fácil de configurar o Code First para usar procedimentos armazenados.

1. Em *DAL\SchoolContext.cs*, adicione o código realçado para o `OnModelCreating` método.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Este código instrui o Entity Framework para procedimentos de uso armazenado para inserir, atualizar e excluir operações no `Department` entidade.
2. No Console de gerenciamento de pacote, digite o seguinte comando:

    `add-migration DepartmentSP`

    Abra *migrações\&lt; timestamp&gt;\_DepartmentSP.cs* para ver o código de `Up` método que cria inserção, atualização e exclusão de procedimentos armazenados:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
- No Console de gerenciamento de pacote, digite o seguinte comando:

    `update-database`
- Executar o aplicativo no modo de depuração, clique no **departamentos** guia e, em seguida, clique em **criar novo**.
- Insira dados para um novo departamento e, em seguida, clique em **criar**.

    ![Criar departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
- No Visual Studio, examine os logs de **saída** janela para ver se um procedimento armazenado foi usado para inserir a nova linha de departamento.

    ![Departamento Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Primeiro, o código cria nomes de procedimento armazenado do padrão. Se você estiver usando um banco de dados existente, talvez seja necessário personalizar os nomes de procedimento armazenado para usar procedimentos armazenados já definidos no banco de dados. Para obter informações sobre como fazer isso, consulte [Entity Framework código primeiro inserir/atualizar/excluir procedimentos armazenados](https://msdn.microsoft.com/data/dn468673).

Se você quiser personalizar quais gerados procedimentos armazenados, você pode editar o código scaffolding para as migrações `Up` método que cria o procedimento armazenado. Assim, suas alterações serão refletidas sempre que a migração é executada e será aplicada ao banco de dados de produção quando migrações é executado automaticamente na produção após a implantação.

Se você quiser alterar um procedimento armazenado existente que foi criado em uma migração anterior, você pode usar o comando Add-Migration para gerar uma migração em branco e escrever manualmente o código que chama o [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) método .

## <a name="deploy-to-azure"></a>Implantar no Azure

Esta seção requer que você concluiu opcional **implantar o aplicativo no Azure** seção o [migrações e implantação](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial desta série. Se você tiver erros de migrações resolvido excluindo o banco de dados em seu projeto local, ignore esta seção.

1. No Visual Studio, clique com botão direito no projeto no **Solution Explorer** e selecione **publicar** no menu de contexto.
2. Clique em **Publicar**.

    O Visual Studio implanta o aplicativo no Azure, e o aplicativo é aberto no navegador padrão, em execução no Azure.
3. Testar o aplicativo para verificar se ela está funcionando.

    Na primeira vez que você executa uma página que acessa o banco de dados, o Entity Framework executa todas as migrações `Up` métodos necessários para colocar o banco de dados atualizado com o modelo de dados atual. Agora você pode usar todas as páginas da web que você adicionou desde a última vez implantado, incluindo as páginas de departamento que você adicionou neste tutorial.

## <a name="summary"></a>Resumo

Neste tutorial você viu como melhorar a eficiência do servidor, escrevendo código que é executado de forma assíncrona e como usar procedimentos armazenados para inserir, atualizar e excluir operações. O seguinte tutorial, você verá como evitar a perda de dados quando vários usuários tentarem editar o mesmo registro ao mesmo tempo.

Links para outros recursos do Entity Framework podem ser encontradas no [acesso a dados ASP.NET - recomendado recursos](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Próximo](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
