---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Validação com os validadores de anotação de dados (VB) | Microsoft Docs
author: microsoft
description: Aproveite o associador de modelo de anotação de dados para executar a validação em um aplicativo ASP.NET MVC. Saiba como usar os diferentes tipos de validador...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: d1987182a44a0ad3f91f455342dc934d1dd50267
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870161"
---
<a name="validation-with-the-data-annotation-validators-vb"></a>Validação com os validadores de anotação de dados (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Aproveite o associador de modelo de anotação de dados para executar a validação em um aplicativo ASP.NET MVC. Saiba como usar os diferentes tipos de atributos de validador e trabalhar com eles no Microsoft Entity Framework.


Neste tutorial, você aprenderá como usar os validadores de anotação de dados para executar a validação em um aplicativo ASP.NET MVC. A vantagem de usar os validadores de anotação de dados é que eles permitem que você executar a validação simplesmente adicionando um ou mais atributos – como obrigatório ou atributo StringLength – para uma propriedade de classe.

Antes de usar os validadores de anotação de dados, você deve baixar o associador de modelo de anotações de dados. Você pode baixar o exemplo de associador de modelo de anotações de dados do site do CodePlex clicando [aqui](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).


É importante entender que o associador de modelo de anotações de dados não é uma parte oficial do Microsoft ASP.NET MVC framework. Embora o associador de modelo de anotações de dados foi criado pela equipe do Microsoft ASP.NET MVC, a Microsoft não oferece suporte oficial do produto para o associador de modelo de anotações de dados descritos e usados neste tutorial.


## <a name="using-the-data-annotation-model-binder"></a>Usando o associador de modelo de anotação de dados

Para usar o associador de modelo de anotações de dados em um aplicativo ASP.NET MVC, primeiro você precisa adicionar uma referência ao assembly Microsoft.Web.Mvc.DataAnnotations.dll e o assembly System.ComponentModel.DataAnnotations.dll. Selecione a opção de menu **projeto, adicionar referência**. Em seguida clique o **procurar** guia e navegue até o local onde você baixou (e descompactado) o exemplo de associador de modelo de anotações de dados (consulte **Figura 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Figura 1**: adicionar uma referência para o associador de modelo de anotações de dados ([clique para exibir a imagem em tamanho normal](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Selecione o assembly Microsoft.Web.Mvc.DataAnnotations.dll e o assembly System.ComponentModel.DataAnnotations.dll o **Okey** botão.


Você não pode usar o assembly System.ComponentModel.DataAnnotations.dll incluído com o .NET Framework Service Pack 1 com o associador de modelo de anotações de dados. Você deve usar a versão do assembly System.ComponentModel.DataAnnotations.dll incluído no download do exemplo de associador de modelo de anotações de dados.


Por fim, você precisa registrar o associador de modelo DataAnnotations no arquivo global. asax. Adicione a seguinte linha de código para o aplicativo\_manipulador de eventos Start () para que o aplicativo\_método Start () tem esta aparência:

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Esta linha de código registra o DataAnnotationsModelBinder como o associador de modelo padrão para todo o aplicativo ASP.NET MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>Usando os atributos de validador de anotação de dados

Quando você usar o associador de modelo de anotações de dados, você usar atributos de validador para executar a validação. O namespace DataAnnotations inclui os seguintes atributos de validação:

- Intervalo – permite que você valide se o valor de uma propriedade fica entre um intervalo de valores especificado.
- ReqularExpression – permite que você valide se o valor de uma propriedade corresponde a um padrão de expressão regular especificada.
- Necessária – permite que você marcar uma propriedade conforme necessário.
- StringLength – permite que você especificar um comprimento máximo de uma propriedade de cadeia de caracteres.
- Validação – a classe base para todos os atributos de validador.

> [!NOTE] 
> 
> Se suas necessidades de validação não são atendidas por qualquer um dos validadores padrão, em seguida, você sempre tem a opção de criação de um atributo de validador personalizado herdando um novo atributo de validador o atributo de validação de base.


A classe do produto em **listagem 1** ilustra como usar esses atributos de validador. As propriedades de nome, descrição e UnitPrice são marcadas conforme necessário. A propriedade de nome deve ter um comprimento de cadeia de caracteres com menos de 10 caracteres. Por fim, a propriedade UnitPrice deve corresponder a um padrão de expressão regular que representa um valor de moeda.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Listando 1**: Models\Product.vb

A classe do produto ilustra como usar um atributo adicional: o atributo DisplayName. O atributo DisplayName permite que você modifique o nome da propriedade quando a propriedade é exibida em uma mensagem de erro. Em vez de exibir a mensagem de erro "o campo UnitPrice é necessário" você pode exibir a mensagem de erro "o campo de preço é obrigatório".

> [!NOTE] 
> 
> Se você quiser personalizar completamente a mensagem de erro exibida por um validador pode atribuir uma mensagem de erro personalizada a propriedade de mensagem de erro do validador assim: `<Required(ErrorMessage:="This field needs a value!")>`


Você pode usar a classe do produto em **listagem 1** com a ação de controlador Create () em **listagem 2**. Esta ação de controlador exibe novamente o Create view quando o estado do modelo contém erros.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**A listagem 2**: Controllers\ProductController.vb

Por fim, você pode criar o modo de exibição **listagem 3** clicando duas vezes a ação Create () e selecionando a opção de menu **adicionar exibição**. Crie uma exibição fortemente tipada com a classe de produto como a classe de modelo. Selecione **criar** na lista suspensa de conteúdo do modo de exibição (consulte **Figura 2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Figura 2**: adicionando Create View

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**A listagem 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Remova o campo Id do formulário Criar gerado pelo **adicionar exibição** opção de menu. Como o campo Id corresponde a uma coluna de identidade, você não quiser para permitir que os usuários insiram um valor para esse campo.


Se você enviar o formulário para a criação de um produto e você não inserir valores para os campos obrigatórios, mensagens de erro de validação no **Figura 3** são exibidos.

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Figura 3**: faltando campos obrigatórios

Se você inserir um valor inválido para moeda, em seguida, a mensagem de erro no **Figura 4** é exibido.

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Figura 4**: valor inválido de moeda

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Usando os validadores de anotação de dados com o Entity Framework

Se você estiver usando o Microsoft Entity Framework para gerar classes de modelo de dados não é possível aplicar os atributos de validador diretamente às classes. Como o Entity Framework Designer gera as classes de modelo, as alterações feitas nas classes de modelo serão substituídas na próxima vez que você fizer alterações no Designer.

Se você quiser usar os validadores com as classes geradas pelo Entity Framework, em seguida, você precisa criar classes de dados de metadados. Você pode aplicar os validadores para a classe de dados de metadados em vez de aplicar os validadores à classe real.

Por exemplo, imagine que você tenha criado uma classe de filme usando o Entity Framework (consulte **Figura 5**). Além disso, imagine que você quer tornar o título do filme e diretor propriedades necessárias. Nesse caso, você pode criar a classe parcial e classe de dados de metadados no **listagem 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Figura 5**: classe de filme gerado pelo Entity Framework

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**A listagem 4**: Models\Movie.vb

O arquivo em **listagem 4** contém duas classes chamadas filme e MovieMetaData. A classe de filme é uma classe parcial. Ele corresponde à classe parcial gerada pelo Entity Framework que está contido no arquivo DataModel.Designer.vb.

Atualmente, o .NET framework não dá suporte a propriedades parciais. Portanto, não há nenhuma maneira de aplicar os atributos de validação para as propriedades da classe filme definido no arquivo DataModel.Designer.vb aplicando os atributos de validação para as propriedades da classe filme definida no arquivo de **listagem 4**.

Observe que a classe parcial de filme é decorada com um atributo MetadataType que aponta para a classe MovieMetaData. A classe MovieMetaData contém propriedades de proxy para as propriedades da classe filme.

Os atributos de validador são aplicados às propriedades da classe MovieMetaData. As propriedades do título, diretor e DateReleased são marcadas como propriedades necessárias. A propriedade Director deve ser atribuída a uma cadeia de caracteres que contenha menos de 5 caracteres. Por fim, o atributo DisplayName é aplicado à propriedade DateReleased para exibir uma mensagem de erro como "o campo de data de lançamento é necessário." em vez do erro "o campo DateReleased é necessário."

> [!NOTE] 
> 
> Observe que as propriedades de proxy na classe MovieMetaData não precisa representar os mesmos tipos de propriedades correspondentes na classe do filme. Por exemplo, a propriedade Director é uma propriedade de cadeia de caracteres na classe de filme e uma propriedade de objeto da classe MovieMetaData.


A página em **Figura 6** ilustra as mensagens de erro retornadas quando você inserir valores inválidos para as propriedades do filme.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Figura 6**: usando validadores com o Entity Framework ([clique para exibir a imagem em tamanho normal](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu como aproveitar o associador de modelo de anotação de dados para executar a validação em um aplicativo ASP.NET MVC. Você aprendeu a usar os diferentes tipos de atributos de validador como necessários e StringLength. Você também aprendeu a usar esses atributos ao trabalhar com o Microsoft Entity Framework.

> [!div class="step-by-step"]
> [Anterior](validating-with-a-service-layer-vb.md)
