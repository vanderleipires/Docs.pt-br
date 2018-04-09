---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Use as migrações do Code First para propagar o banco de dados | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 33bc6d82daa9ca5f46452a1adf4e2eebea04fa6c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Use as migrações do Code First para propagar o banco de dados
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você usará [migrações do Code First](https://msdn.microsoft.com/data/jj591621) no EF para propagar o banco de dados de teste.

Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](part-3/samples/sample1.cmd)]

Este comando adiciona uma pasta chamada migrações ao seu projeto, além de um arquivo de código chamado Configuration.cs na pasta de migrações.

![](part-3/_static/image1.png)

Abra o arquivo Configuration.cs. Adicione o seguinte **usando** instrução.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Em seguida, adicione o seguinte código para o **Configuration.Seed** método:

[!code-csharp[Main](part-3/samples/sample3.cs)]

Na janela do Console do Gerenciador de pacotes, digite os seguintes comandos:

[!code-console[Main](part-3/samples/sample4.cmd)]

O primeiro comando gera o código que cria o banco de dados, e o segundo comando executa esse código. O banco de dados é criado localmente, usando [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Explorar a API (opcional)

Pressione F5 para executar o aplicativo no modo de depuração. O Visual Studio inicia o IIS Express e executa seu aplicativo web. Visual Studio, em seguida, inicia um navegador e abre a home page do aplicativo.

Quando o Visual Studio executará um projeto da web, ele atribui um número de porta. Na imagem abaixo, o número da porta é 50524. Quando você executa o aplicativo, você verá um número de porta diferente.

![](part-3/_static/image3.png)

A home page é implementada usando o ASP.NET MVC. Na parte superior da página, há um link que diz "API". Este link traz uma página de ajuda gerada automaticamente para a API da web. (Para saber como esta página de Ajuda é gerada e como você pode adicionar sua própria documentação para a página, consulte [criando páginas de ajuda para a ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Você pode clicar no menu Ajuda links da página para ver os detalhes sobre a API, incluindo o formato de solicitação e resposta.

![](part-3/_static/image4.png)

A API permite operações CRUD no banco de dados. A seguir resume a API.

| Autores |  |
| --- | -- |
| OBTER api/autores | Obter todos os autores. |
| Api GET/autores / {id} | Obter um autor por ID. |
| Autores/api/POST | Crie um novo autor. |
| COLOCAR /api autores / {id} | Atualize um autor existente. |
| Excluir /api autores / {id} | Exclua um autor. |

| Livros |  |
| --- | -- |
| OBTER /api/books | Obter todos os livros. |
| OBTER /api manuais / {id} | Obter um livro por ID. |
| Enviar/api/catálogos | Crie um novo catálogo. |
| COLOCAR /api manuais / {id} | Atualize um catálogo existente. |
| Excluir /api manuais / {id} | Exclua um catálogo. |

## <a name="view-the-database-optional"></a>Exibir o banco de dados (opcional)

Quando você executou o comando Update-Database, EF criou o banco de dados e a chamada a `Seed` método. Quando você executar o aplicativo localmente, usa EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Você pode exibir o banco de dados no Visual Studio. Do **exibição** menu, selecione **Pesquisador de objetos do SQL Server**.

![](part-3/_static/image5.png)

No **conectar ao servidor** caixa de diálogo, no **nome do servidor** caixa de edição, digite "\v11.0 (localdb)". Deixe o **autenticação** opção como "Autenticação do Windows". Clique em **Conectar**.

![](part-3/_static/image6.png)

Visual Studio se conecta ao LocalDB e mostra os bancos de dados existentes na janela do Pesquisador de objetos do SQL Server. Você pode expandir os nós para ver as tabelas que EF criada.

![](part-3/_static/image7.png)

Para exibir os dados, clique em uma tabela e selecione **exibir dados**.

![](part-3/_static/image8.png)

Captura de tela a seguir mostra os resultados da tabela livros. Observe que o EF populado o banco de dados com os dados de semente, e a tabela contém a chave estrangeira para a tabela de autores.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Anterior](part-2.md)
> [Próximo](part-4.md)
