---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Adicionar modelos e controladores | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 162ef2cd4ba11040e1bc938617a36495489ba5bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830425"
---
<a name="add-models-and-controllers"></a>Adicionar modelos e controladores
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você irá adicionar classes de modelo que definem as entidades de banco de dados. Em seguida, você irá adicionar controladores da API Web que executam operações CRUD nessas entidades.

## <a name="add-model-classes"></a>Adicionar Classes de modelo

Neste tutorial, vamos criar o banco de dados usando a abordagem de "Code First" para o Entity Framework (EF). Com o Code First, escrever classes em c# que correspondem às tabelas de banco de dados e o EF cria o banco de dados. (Para obter mais informações, consulte [abordagens de desenvolvimento do Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Vamos começar definindo nossos objetos de domínio como POCOs (objetos CLR do BOM e velho). Vamos criar POCOs a seguir:

- Autor
- Livro

No Gerenciador de soluções, clique com botão direito na pasta modelos. Selecione **Add**, em seguida, selecione **classe**. Nomeie a classe `Author`.

![](part-2/_static/image1.png)

Substitua todo o código clichê em Author.cs com o código a seguir.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Adicione outra classe denominada `Book`, com o código a seguir.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework usará esses modelos para criar tabelas de banco de dados. Para cada modelo, o `Id` propriedade tornam-se a coluna de chave primária da tabela de banco de dados.

Na classe do livro, o `AuthorId` define uma chave estrangeira para a `Author` tabela. (Para simplificar, estou pressupondo que cada livro tem o autor de um único.) A classe de livro também contém uma propriedade de navegação para o relacionado `Author`. Você pode usar a propriedade de navegação para acessar o relacionados `Author` no código. Posso falar mais sobre as propriedades de navegação na parte 4, [tratamento de relações de entidade](part-4.md).

## <a name="add-web-api-controllers"></a>Adicionar controladores de API da Web

Nesta seção, vamos adicionar controladores da API Web que dão suporte a operações CRUD (criar, ler, atualizar e excluir). Os controladores usará o Entity Framework para se comunicar com a camada de banco de dados.

Primeiro, você pode excluir o arquivo Controllers/ValuesController.cs. Esse arquivo contém um controlador de API Web de exemplo, mas ele não é necessário para este tutorial.

![](part-2/_static/image2.png)

Em seguida, compile o projeto. O scaffolding de API da Web usa a reflexão para encontrar as classes de modelo, portanto, ele precisa que o assembly compilado.

No Gerenciador de soluções, clique com botão direito na pasta controladores. Selecione **Add**, em seguida, selecione **controlador**.

![](part-2/_static/image3.png)

No **adicionar Scaffold** caixa de diálogo, selecione "Web API 2 Controller com ações, usando o Entity Framework". Clique em **Adicionar**.

![](part-2/_static/image4.png)

No **Adicionar controlador** caixa de diálogo, faça o seguinte:

1. No **classe de modelo** lista suspensa, selecione o `Author` classe. (Se você não vi-lo listado na lista suspensa, verifique se que você compilou o projeto.)
2. Verifique se "Use async controller actions".
3. Deixe o nome do controlador como &quot;AuthorsController&quot;.
4. Clique em mais (+) ao lado de botão **classe de contexto de dados**.

![](part-2/_static/image5.png)

No **novo contexto de dados** caixa de diálogo, deixe o nome padrão e clique em **Add**.

![](part-2/_static/image6.png)

Clique em **Add** para concluir a **Adicionar controlador** caixa de diálogo. A caixa de diálogo adiciona duas classes ao seu projeto:

- `AuthorsController` define um controlador de API da Web. O controlador implementa a API REST que os clientes usam para executar operações CRUD, na lista de autores.
- `BookServiceContext` gerencia objetos de entidade durante o tempo de execução, que inclui a população de objetos com dados de um banco de dados, o controle de alterações e a persistência de dados para o banco de dados. Herda de `DbContext`.

![](part-2/_static/image7.png)

Neste ponto, compile o projeto novamente. Agora, percorra as mesmas etapas para adicionar um controlador de API para `Book` entidades. Desta vez, selecione `Book` para a classe de modelo e selecione existente `BookServiceContext` classe para a classe de contexto de dados. (Não crie um novo contexto de dados). Clique em **adicionar** para adicionar o controlador.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Anterior](part-1.md)
> [Próximo](part-3.md)
