---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Validação do modelo na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/20/2012
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 611a6466e160387592df678b3b8556625ff8e234
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829982"
---
<a name="model-validation-in-aspnet-web-api"></a>Validação do modelo na API Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Quando um cliente envia dados para sua API da web, muitas vezes você deseja validar os dados antes de fazer qualquer processamento. Este artigo mostra como anotar seus modelos, use as anotações para validação de dados e tratar erros de validação em sua API da web.

## <a name="data-annotations"></a>Anotações de dados

Na API Web ASP.NET, você pode usar atributos do [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace para definir regras de validação para propriedades em seu modelo. Considere o seguinte modelo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Se você tiver usado a validação do modelo no ASP.NET MVC, isso deve ser familiar. O **necessária** atributo diz que o `Name` propriedade não pode ser nula. O **intervalo** atributo diz que `Weight` deve estar entre zero e 999.

Suponha que um cliente envia uma solicitação POST com a seguinte representação JSON:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Você pode ver que o cliente não incluiu o `Name` propriedade, que é marcada como obrigatório. Quando a API da Web converte o JSON em uma `Product` instância, ele valida o `Product` contra os atributos de validação. A ação de controlador, você pode verificar se o modelo é válido:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Validação de modelo não garante que os dados de cliente estão seguros. Validação adicional pode ser necessário em outras camadas do aplicativo. (Por exemplo, a camada de dados pode impor restrições de chave estrangeira.) O tutorial [usando a API da Web com o Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explora alguns desses problemas.

**"Corrigiremos lançamento"**: lançamento corrigiremos acontece quando o cliente omite algumas propriedades. Por exemplo, suponha que o cliente envia o seguinte:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Aqui, o cliente não especificou valores para `Price` ou `Weight`. O formatador JSON atribui um valor padrão de zero para as propriedades ausentes.

![](model-validation-in-aspnet-web-api/_static/image1.png)

O estado do modelo é válido, porque o zero é um valor válido para essas propriedades. Se esse é um problema depende do seu cenário. Por exemplo, em uma operação de atualização, convém distinguir entre "zero" e "não definido". Para forçar os clientes para definir um valor, verifique a propriedade anulável e defina as **necessário** atributo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Excesso de postagem"**: um cliente também pode enviar *mais* dados que o esperado. Por exemplo:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Aqui, o JSON inclui uma propriedade ("Color") que não existe no `Product` modelo. Nesse caso, o formatador JSON simplesmente ignora esse valor. (O formatador XML faz o mesmo.) Excesso de postagem causa problemas se seu modelo tem propriedades que devem ser somente leitura. Por exemplo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Você não deseja que os usuários atualizem o `IsAdmin` propriedade e elevar a mesmos para os administradores! A estratégia mais segura é usar uma classe de modelo que corresponda exatamente o que o cliente tem permissão para enviar:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Postagem de blog de Brad Wilson "[vs de validação de entrada. Modelo de validação no ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"tem uma boa discussão do lançamento insuficiente e excesso de postagem. Embora a postagem é sobre o ASP.NET MVC 2, os problemas ainda são relevantes para a API Web.


## <a name="handling-validation-errors"></a>Tratamento de erros de validação

API da Web não automaticamente retornar um erro ao cliente quando a validação falha. Cabe a ação do controlador para verificar o estado do modelo e responder adequadamente.

Você também pode criar um filtro de ação para verificar o estado do modelo antes que a ação do controlador seja invocada. O código a seguir mostra um exemplo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Se a validação do modelo falhar, esse filtro retorna uma resposta HTTP que contém os erros de validação. Nesse caso, a ação do controlador não é invocada.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Para aplicar esse filtro para todos os controladores de API da Web, adicione uma instância do filtro para o **HttpConfiguration.Filters** coleção durante a configuração:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Outra opção é definir o filtro como um atributo em controladores ou ações do controlador individuais:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
