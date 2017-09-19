---
title: "Associação de modelo personalizado"
author: ardalis
description: "Personalizando a associação de modelo no ASP.NET MVC de núcleo."
keywords: "ASP.NET Core, associação de modelo, o associador de modelo personalizado"
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.assetid: ebd98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 999c4f76354ef6ce07733bbb8b0094be90c03c26
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2017
---
# <a name="custom-model-binding"></a>Associação de modelo personalizado

Por [Steve Smith](https://ardalis.com/)

Associação de modelo permite que as ações do controlador trabalhar diretamente com os tipos de modelo (transmitidos como argumentos de método), em vez de solicitações HTTP. Mapeamento entre modelos de dados e aplicativos de solicitação entrados é tratado pelo associadores de modelo. Os desenvolvedores podem estender a funcionalidade de associação de modelo interno implementando associadores de modelo personalizado (embora normalmente, você não precisa escrever seu próprio provedor).

[Exibir ou baixar o exemplo do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Limitações de associador de modelo padrão

Os associadores de modelo padrão dão suporte à maioria dos tipos de dados comuns do .NET Core e devem atender às necessidades da maioria dos desenvolvedores. Esperam associar o texto de entrada da solicitação diretamente a tipos de modelo. Talvez seja necessário transformar a entrada antes de associar a ele. Por exemplo, se você tiver uma chave que pode ser usada para pesquisar dados de modelo. Você pode usar um associador de modelos personalizados para buscar dados com base na chave.

## <a name="model-binding-review"></a>Análise de associação de modelo

Associação de modelo usa definições específicas para os tipos que ela opera. Um *tipo simples* é convertido de uma única cadeia de caracteres na entrada. Um *tipo complexo* é convertido de vários valores de entrada. A estrutura determina a diferença com base na existência de um `TypeConverter`. É recomendável que você criar um conversor de tipo, se você tiver um simples `string`  ->  `SomeType` mapeamento que não exige recursos externos.

Antes de criar seu próprio associador de modelo personalizado, é vale a pena modelo existente como revisão associadores são implementadas. Considere o [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) que pode ser usado para converter cadeias de caracteres codificada em base64 em matrizes de bytes. As matrizes de bytes geralmente são armazenadas como arquivos ou campos do banco de dados BLOB.

### <a name="working-with-the-bytearraymodelbinder"></a>Trabalhando com o ByteArrayModelBinder

Cadeias de caracteres codificada em Base64 podem ser usadas para representar dados binários. Por exemplo, a imagem a seguir pode ser codificada como uma cadeia de caracteres.

![dotnet bot](custom-model-binding/images/bot.png "bot dotnet")

Uma pequena parte da cadeia de caracteres codificada é mostrada na imagem a seguir:

![dotnet bot codificado](custom-model-binding/images/encoded-bot.png "bot dotnet codificado")

Siga as instruções de [Leiame do exemplo](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) para converter a cadeia de caracteres codificada em base64 em um arquivo.

ASP.NET MVC de núcleo pode levar um cadeias de caracteres codificada em base64 e usar um `ByteArrayModelBinder` para convertê-la em uma matriz de bytes. O [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) que implementa [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mapeia `byte[]` argumentos para `ByteArrayModelBinder`:

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

Ao criar seu próprio associador de modelo personalizado, você pode implementar seu próprio `IModelBinderProvider` digitar ou usar o [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).

O exemplo a seguir mostra como usar `ByteArrayModelBinder` para converter uma cadeia de caracteres codificada em base64 para um `byte[]` e salvar o resultado em um arquivo:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

Você pode lançar uma cadeia de caracteres codificada em base64 para esse método de api usando uma ferramenta como [carteiro](https://www.getpostman.com/):

![carteiro](custom-model-binding/images/postman.png "carteiro")

Como o associador pode associar dados de solicitação para propriedades nomeadas adequadamente ou argumentos, associação de modelo terá êxito. O exemplo a seguir mostra como usar `ByteArrayModelBinder` com um modelo de exibição:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Exemplo de associador de modelo personalizado

Nesta seção, implementaremos um associador de modelo personalizado que:

- Converte dados de solicitação de entrada em argumentos de chave fortemente tipados.
- Usa o Entity Framework Core para buscar a entidade associada.
- Passa a entidade associada como um argumento para o método de ação.

O exemplo a seguir usa o `ModelBinder` atributo no `Author` modelo:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

No código anterior, o `ModelBinder` atributo especifica o tipo de `IModelBinder` que deve ser usado para associar `Author` parâmetros de ação. 

O `AuthorEntityBinder` é usado para associar um `Author` parâmetro buscando a entidade de uma fonte de dados usando o Entity Framework Core e uma `authorId`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

O código a seguir mostra como usar o `AuthorEntityBinder` em um método de ação:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

O `ModelBinder` atributo pode ser usado para aplicar o `AuthorEntityBinder` aos parâmetros que não usam convenções padrão:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

Neste exemplo, como o nome do argumento não é o padrão `authorId`, ele é especificado no parâmetro usando `ModelBinder` atributo. Observe que o controlador e ação de método são simplificadas comparado ao pesquisar a entidade no método de ação. A lógica para buscar o autor usando o Entity Framework Core é movida para o associador de modelo. Isso pode ser considerável simplificação quando há vários métodos que ligar para o modelo de autor e podem ajudá-lo a seguir o [princípio seco](http://deviq.com/don-t-repeat-yourself/).

Você pode aplicar o `ModelBinder` às propriedades de modelo individuais de atributos (como em um viewmodel) ou para parâmetros de método de ação para especificar um determinado associador de modelo ou nome de modelo para apenas esse tipo ou a ação.

### <a name="implementing-a-modelbinderprovider"></a>Implementando um ModelBinderProvider

Em vez de aplicar um atributo, você pode implementar `IModelBinderProvider`. Isso é como os associadores de estrutura interna são implementados. Quando você especifica o tipo de seu fichário opera em, especifique o tipo de argumento produz, **não** a entrada seu fichário aceita. O provedor de associador seguir funciona com o `AuthorEntityBinder`. Quando ele é adicionado à coleção do MVC de provedores, você não precisa usar o `ModelBinder` atributo no `Author` ou `Author` parâmetros digitados.

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Observação: O código anterior retorna um `BinderTypeModelBinder`. `BinderTypeModelBinder`funciona como uma fábrica de associadores de modelo e fornece a injeção de dependência (DI). O `AuthorEntityBinder` requer DI acessem EF Core. Use `BinderTypeModelBinder` se o associador de modelo requer serviços de injeção de dependência.

Para usar um provedor de associador de modelo personalizado, adicione-o em `ConfigureServices`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Ao avaliar os associadores de modelo, a coleção de provedores é examinada em ordem. O primeiro provedor que retorna um associador é usado.

A imagem a seguir mostra o padrão de associadores de modelo do depurador.

![padrão de associadores de modelo](custom-model-binding/images/default-model-binders.png "padrão associadores de modelo")

Adicionar o provedor ao final da coleção pode resultar em um associador de modelos internos que está sendo chamado antes de seu fichário personalizado tem uma possibilidade. Neste exemplo, o provedor personalizado é adicionado ao início da coleção para garantir que ele é usado para `Author` argumentos de ação.

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>Recomendações e práticas recomendadas

Associadores de modelo personalizado:
- Não tente definir códigos de status ou retornar resultados (por exemplo, 404 não encontrado). Se a associação de modelo falhar, um [filtro de ação](xref:mvc/controllers/filters) ou lógica dentro do método de ação deve lidar com falhas.
- São mais úteis para eliminar código repetitivo e resolvem preocupações de métodos de ação.
- Normalmente não deve ser usado para converter uma cadeia de caracteres em um tipo personalizado, uma [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) geralmente é uma opção melhor.
