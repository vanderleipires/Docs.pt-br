---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: Criar um controlador de Admin | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a>Parte 3: Criar um controlador de Admin
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Adicionar um controlador de Admin

Nesta seção, vamos adicionar um controlador de API da Web que oferece suporte a CRUD (criar, ler, atualizar e excluir) operações em produtos. O controlador usará o Entity Framework para se comunicar com a camada de banco de dados. Somente os administradores poderão usar esse controlador. Os clientes acessarão os produtos por meio de outro controlador.

No Gerenciador de soluções, clique na pasta de controladores. Selecione **adicionar** e **controlador**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

No **Adicionar controlador** caixa de diálogo, o nome do controlador `AdminController`. Em **modelo**, selecione &quot;controlador API com ações de leitura/gravação, usando o Entity Framework&quot;. Em **classe modelo**, selecione "Product (ProductStore.Models)". Em **contexto de dados**, selecione "&lt;novo contexto de dados&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Se o **classe modelo** suspensa não mostra as classes de modelo, verifique se você compilou o projeto. Entity Framework usa reflexão, então ele precisa do assembly compilado.


Selecionando "&lt;novo contexto de dados&gt;" abrirá o **novo contexto de dados** caixa de diálogo. Nomeie o contexto de dados `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Clique em **Okey** para ignorar o **novo contexto de dados** caixa de diálogo. No **Adicionar controlador** caixa de diálogo, clique em **adicionar**.

Aqui está o que foi adicionado ao projeto:

- Uma classe denominada `OrdersContext` que deriva de **DbContext**. Essa classe fornece a cola entre os modelos POCO e o banco de dados.
- Um controlador de API da Web chamado `AdminController`. Esse controlador suporta operações CRUD em `Product` instâncias. Ele usa o `OrdersContext` classe para se comunicar com o Entity Framework.
- Uma nova cadeia de conexão de banco de dados no arquivo Web. config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Abra o arquivo OrdersContext.cs. Observe que o construtor Especifica o nome da cadeia de caracteres de conexão de banco de dados. Esse nome refere-se à cadeia de conexão que foi adicionada ao Web. config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Adicione as seguintes propriedades à classe `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Um **DbSet** representa um conjunto de entidades que podem ser consultados. Aqui está a lista completa de `OrdersContext` classe:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

O `AdminController` classe define cinco métodos que implementam a funcionalidade básica de CRUD. Cada método corresponde a um URI que o cliente pode invocar:

| Método do controlador | Descrição | URI | Método HTTP |
| --- | --- | --- | --- |
| GetProducts | Obtém todos os produtos. | API e produtos | OBTER |
| GetProduct | Localiza um produto por ID. | api/products/*id* | OBTER |
| PutProduct | Atualiza um produto. | api/products/*id* | PUT |
| PostProduct | Cria um novo produto. | API e produtos | POSTAR |
| DeleteProduct | Exclui um produto. | api/products/*id* | DELETE |

Cada método chama `OrdersContext` para consultar o banco de dados. Chamam os métodos que modificam a coleção (PUT, POST e DELETE) `db.SaveChanges` para manter as alterações no banco de dados. Os controladores são criados por solicitação HTTP e, em seguida, descartados, portanto, é necessário manter as alterações antes de retorna de um método.

## <a name="add-a-database-initializer"></a>Adicionar um inicializador de banco de dados

Entity Framework tem um recurso interessante que permite que você preencher o banco de dados na inicialização e recriar automaticamente o banco de dados sempre que alterar os modelos. Esse recurso é útil durante o desenvolvimento, porque você sempre ter alguns dados de teste, mesmo se você alterar os modelos.

No Gerenciador de soluções, clique com botão direito na pasta modelos e criar uma nova classe chamada `OrdersContextInitializer`. Cole a seguinte implementação:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Herdando do **DropCreateDatabaseIfModelChanges** classe, dizemos Entity Framework para descartar o banco de dados sempre que é modificar as classes de modelo. Quando o Entity Framework cria (ou recria) o banco de dados, ele chama o **semente** para popular as tabelas. Usamos o **semente** método para adicionar alguns produtos de exemplo e uma ordem de exemplo.

Esse recurso é ótimo para teste, mas não use o **DropCreateDatabaseIfModelChanges** classe em produção, pois você pode perder os dados se alguém altera uma classe de modelo.

Em seguida, abra global. asax e adicione o seguinte código para o **aplicativo\_iniciar** método:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Enviar uma solicitação para o controlador

Neste momento, não tenhamos escrito qualquer código de cliente, mas você pode chamar web API usando um navegador da web ou uma depuração de HTTP, como ferramenta [Fiddler](http://www.fiddler2.com/fiddler2/). No Visual Studio, pressione F5 para iniciar a depuração. Seu navegador da web será aberto para `http://localhost:*portnum*/`, onde *portnum* é um número de porta.

Enviar uma solicitação HTTP para "`http://localhost:*portnum*/api/admin`. A primeira solicitação pode ser lenta para ser concluída, porque a estrutura Entify precisa para criar e propagar o banco de dados. A resposta deve algo semelhante ao seguinte:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-2.md)
> [Próximo](using-web-api-with-entity-framework-part-4.md)
