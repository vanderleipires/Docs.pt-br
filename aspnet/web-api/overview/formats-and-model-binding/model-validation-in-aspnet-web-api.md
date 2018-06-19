---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Modelo de validação na API da Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 409a91eceb8baa48a7dded1b850d59a27cec2c60
ms.sourcegitcommit: 5ae0c125ee3bbd324edef3818d1d160f4dd84602
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34224720"
---
<a name="model-validation-in-aspnet-web-api"></a>Validação de modelo na API da Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Quando um cliente envia dados para sua API da web, geralmente você deseja validar os dados antes de executar qualquer processamento. Este artigo mostra como anotar seus modelos, use as anotações para validação de dados e tratar erros de validação em sua API da web.

## <a name="data-annotations"></a>Anotações de dados

Na API da Web do ASP.NET, você pode usar os atributos de [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace para definir as regras de validação para propriedades em seu modelo. Considere o seguinte modelo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Se você tiver usado a validação do modelo no ASP.NET MVC, isso deve parecer familiar. O **necessário** atributo diz que a `Name` propriedade não pode ser nula. O **intervalo** atributo diz que `Weight` deve ser entre 0 e 999.

Suponha que um cliente envia uma solicitação POST com a representação de JSON a seguir:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Você pode ver que o cliente não incluiu a `Name` propriedade, que é marcada como necessária. Quando a API da Web converte o JSON em uma `Product` instância, ele valida o `Product` contra os atributos de validação. A ação de controlador, você pode verificar se o modelo é válido:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Validação de modelo não garante a seguros de dados do cliente. A validação adicional pode ser necessário em outras camadas do aplicativo. (Por exemplo, a camada de dados pode impor restrições de chave estrangeira.) O tutorial [usando API da Web com o Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explora alguns desses problemas.

**"Lançamento insuficiente"**: lançamento insuficiente acontece quando o cliente omite algumas propriedades. Por exemplo, suponha que o cliente envia o seguinte:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Aqui, o cliente não especificou valores para `Price` ou `Weight`. O formatador JSON atribui um valor padrão de zero para as propriedades ausentes.

![](model-validation-in-aspnet-web-api/_static/image1.png)

O estado do modelo é válido, pois o zero é um valor válido para essas propriedades. Se esse é um problema depende do seu cenário. Por exemplo, em uma operação de atualização, convém distinguir entre "zero" e "não configurado". Para forçar os clientes para definir um valor, verifique a propriedade anulável e defina o **necessário** atributo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Excesso de lançamento"**: um cliente também pode enviar *mais* dados que o esperado. Por exemplo:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Aqui, o JSON inclui uma propriedade ("cores") que não existe no `Product` modelo. Nesse caso, o formatador JSON simplesmente ignora esse valor. (O formatador XML faz o mesmo.) Excesso de lançamento causa problemas, se seu modelo tem propriedades que devem ser somente leitura. Por exemplo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Você não deseja que os usuários atualizem o `IsAdmin` propriedade e elevar os próprios administradores! A estratégia mais segura é usar uma classe de modelo que corresponda exatamente o que o cliente tem permissão para enviar:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Postagem do blog de Brad Wilson "[vs de validação de entrada. Modelo de validação no ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"tem uma boa discussão do lançamento insuficiente e excesso de lançamento. Embora a postagem é sobre o ASP.NET MVC 2, os problemas ainda são relevantes para a API da Web.


## <a name="handling-validation-errors"></a>Tratamento de erros de validação

API da Web não automaticamente retornar um erro para o cliente quando a validação falha. Cabe a ação de controlador para verificar o estado do modelo e responder adequadamente.

Você também pode criar um filtro de ação para verificar o estado do modelo antes da ação de controlador é invocada. O código a seguir mostra um exemplo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Se a validação do modelo falhar, esse filtro retorna uma resposta HTTP que contém os erros de validação. Nesse caso, a ação de controlador não é invocada.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Para aplicar esse filtro para todos os controladores de API da Web, adicione uma instância do filtro para o **HttpConfiguration.Filters** coleção durante a configuração:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Outra opção é definir o filtro como um atributo de controladores individuais ou ações do controlador:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
