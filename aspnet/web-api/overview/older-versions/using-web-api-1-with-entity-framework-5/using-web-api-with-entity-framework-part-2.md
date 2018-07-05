---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: Criando modelos de domínio | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 605a3eb5d781db6b317369efe8698288d7b9f648
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802264"
---
<a name="part-2-creating-the-domain-models"></a>Parte 2: Criando modelos de domínio
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Adicionar modelos

Há três maneiras de abordagem do Entity Framework:

- Banco de dados, primeiro: iniciar com um banco de dados e o Entity Framework gera o código.
- Model first: iniciar com um modelo de visual e o Entity Framework gera o banco de dados e o código.
- Código primeiro: iniciar com o código e o Entity Framework gera o banco de dados.

Estamos usando a abordagem de primeiro código, portanto, começamos definindo nossos objetos de domínio como POCOs (objetos CLR do BOM e velho). Com a abordagem de primeiro código, objetos de domínio não é necessário nenhum código extra para dar suporte à camada de banco de dados, como transações ou persistência. (Especificamente, eles não precisam herdar de [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) classe.) Você ainda pode usar anotações de dados para controlar como o Entity Framework cria o esquema de banco de dados.

Porque POCOs não transmitem quaisquer propriedades adicionais que descrevem [estado de banco de dados](https://msdn.microsoft.com/library/system.data.entitystate.aspx), eles possam facilmente ser serializados para JSON ou XML. No entanto, isso não significa que você sempre deve expor seus modelos do Entity Framework diretamente aos clientes, como veremos posteriormente no tutorial.

Vamos criar POCOs a seguir:

- Produto
- Pedido
- OrderDetail

Para criar cada classe, clique com botão direito na pasta de modelos no Gerenciador de soluções. No menu de contexto, selecione **Add** e, em seguida, selecione **classe.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Adicionar um `Product` classe com a implementação a seguir:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Por convenção, o Entity Framework usa o `Id` a propriedade como a chave primária e a mapeia em uma coluna de identidade na tabela de banco de dados. Quando você cria um novo `Product` instância, você não precisará configurar um valor para `Id`, porque o banco de dados gera o valor.

O **ScaffoldColumn** atributo diz ao ASP.NET MVC para ignorar o `Id` ao gerar um formulário de editor de propriedade. O **necessário** atributo é usado para validar o modelo. Especifica que o `Name` propriedade deve ser uma cadeia de caracteres não vazia.

Adicionar o `Order` classe:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Adicionar o `OrderDetail` classe:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relações de chave estrangeiras

Um pedido contém muitos detalhes do pedido, e cada detalhe do pedido se refere a um único produto. Para representar essas relações, o `OrderDetail` classe define as propriedades nomeadas `OrderId` e `ProductId`. Entity Framework irá inferir que essas propriedades representam chaves estrangeiras e adicionará as restrições foreign key no banco de dados.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

O `Order` e `OrderDetail` classes também incluem as propriedades de "navegação", que contêm referências a objetos relacionados. Dada uma ordem, você pode navegar para os produtos na ordem, seguindo as propriedades de navegação.

Compile o projeto agora. Entity Framework usa a reflexão para descobrir as propriedades dos modelos, portanto, ele exige um assembly compilado criar o esquema de banco de dados.

## <a name="configure-the-media-type-formatters"></a>Configurar os formatadores de tipo de mídia

Um [formatador do tipo de mídia](../../formats-and-model-binding/media-formatters.md) é um objeto que serializa os dados quando a API da Web grava o corpo da resposta HTTP. Os formatadores internos dão suporte a saída JSON e XML. Por padrão, ambas essas formatadores serializar todos os objetos por valor.

Serialização por valor cria um problema se um gráfico de objeto contém referências circulares. Isso é exatamente o caso com o `Order` e `OrderDetail` classes, porque cada um contém uma referência para o outro. O formatador siga as referências a gravação de cada objeto por valor e ir em círculos. Portanto, precisamos alterar o comportamento padrão.

No Gerenciador de soluções, expanda o aplicativo\_iniciar pasta e abra o arquivo chamado WebApiConfig.cs. Adicione o seguinte código à classe `WebApiConfig`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Esse código define o formatador JSON para preservar as referências de objeto e remove o formatador XML do pipeline totalmente. (Você pode configurar o formatador XML para preservar as referências de objeto, mas é um pouco mais de trabalho e só precisamos JSON para este aplicativo. Para obter mais informações, consulte [tratamento referências circulares do objeto](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-1.md)
> [Próximo](using-web-api-with-entity-framework-part-3.md)
