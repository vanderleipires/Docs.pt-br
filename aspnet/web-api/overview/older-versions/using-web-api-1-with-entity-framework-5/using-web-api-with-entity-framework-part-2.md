---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: "Parte 2: Criando modelos de domínio | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: a573b47d27767dc78d557cd2b6c73714eb9e94f4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="part-2-creating-the-domain-models"></a>Parte 2: Criando modelos de domínio
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Adicionar modelos

Há três maneiras de abordagem do Entity Framework:

- Banco de dados, primeiro: iniciar com um banco de dados e gera o código de Entity Framework.
- Modelo primeiro: iniciar com um modelo de visual, e o Entity Framework gera o banco de dados e o código.
- Código primeiro: iniciar com o código e o Entity Framework gera o banco de dados.

Estamos usando a abordagem de código, portanto começamos definindo nossos objetos de domínio como POCOs (objetos CLR antigo simples). Com a abordagem de código, objetos de domínio não é necessário nenhum código adicional para dar suporte a camada de banco de dados, como transações ou persistência. (Especificamente, eles não precisa herdar o [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) classe.) Você ainda pode usar anotações de dados para controlar como o Entity Framework cria o esquema de banco de dados.

Porque o POCOs não contém quaisquer propriedades adicionais que descrevem [estado do banco de dados](https://msdn.microsoft.com/library/system.data.entitystate.aspx), facilmente pode ser serializados para JSON ou XML. No entanto, isso não significa você sempre deve expor seus modelos do Entity Framework diretamente aos clientes, como você verá posteriormente no tutorial.

Vamos criar POCOs a seguir:

- Produto
- Pedido
- OrderDetail

Para criar cada classe, clique na pasta de modelos no Gerenciador de soluções. No menu de contexto, selecione **adicionar** e, em seguida, selecione **classe.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Adicionar um `Product` classe com a implementação a seguir:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Por convenção, o Entity Framework usa o `Id` a propriedade como a chave primária e mapeia para uma coluna de identidade na tabela de banco de dados. Quando você cria um novo `Product` instância, você não definir um valor para `Id`, porque o banco de dados gera o valor.

O **ScaffoldColumn** atributo informa ao ASP.NET MVC para ignorar o `Id` ao gerar um formulário do editor de propriedade. O **necessário** atributo é usado para validar o modelo. Especifica que o `Name` propriedade deve ser uma cadeia de caracteres não vazia.

Adicionar o `Order` classe:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Adicionar o `OrderDetail` classe:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relações de chave estrangeiras

Um pedido contém muitos detalhes do pedido e cada detalhe do pedido se refere a um único produto. Para representar essas relações, o `OrderDetail` classe define propriedades chamadas `OrderId` e `ProductId`. Entity Framework deduzirá que essas propriedades representam as chaves estrangeiras e adicionará as restrições foreign key no banco de dados.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

O `Order` e `OrderDetail` classes também incluem propriedades de "navegação", que contêm referências a objetos relacionados. Dada uma ordem, você pode navegar para os produtos na ordem, seguindo as propriedades de navegação.

Compile o projeto agora. Entity Framework usa reflexão para descobrir as propriedades dos modelos, isso requer um assembly compilado criar o esquema de banco de dados.

## <a name="configure-the-media-type-formatters"></a>Configurar os formatadores de tipo de mídia

Um [formatador do tipo de mídia](../../formats-and-model-binding/media-formatters.md) é um objeto que serializa os dados quando a API da Web grava o corpo da resposta HTTP. Os formatadores internos oferecem suporte a saída JSON e XML. Por padrão, ambos esses formatadores serializar todos os objetos por valor.

Serialização por valor cria um problema se um gráfico de objeto contém referências circulares. Esse é exatamente o caso com o `Order` e `OrderDetail` classes, porque cada uma contém uma referência para o outro. O formatador siga as referências, writing cada objeto de valor e ir em círculos. Portanto, é preciso alterar o comportamento padrão.

No Gerenciador de soluções, expanda o aplicativo\_pasta e abra o arquivo chamado WebApiConfig.cs. Adicione o seguinte código à classe `WebApiConfig`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Esse código define o formatador JSON para preservar as referências de objeto e remove o formatador XML do pipeline inteiramente. (Você pode configurar o formatador XML para preservar as referências de objeto, mas é um pouco mais trabalho e precisamos apenas JSON para este aplicativo. Para obter mais informações, consulte [tratamento de referências de objeto Circular](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

>[!div class="step-by-step"]
[Anterior](using-web-api-with-entity-framework-part-1.md)
[Próximo](using-web-api-with-entity-framework-part-3.md)
