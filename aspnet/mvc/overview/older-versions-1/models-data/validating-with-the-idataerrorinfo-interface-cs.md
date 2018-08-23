---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Validação com a Interface IDataErrorInfo (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther mostra como exibir mensagens de erro de validação personalizada, Implementando a interface IDataErrorInfo em uma classe de modelo.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b80535db32c4567135407aeb99967bb40c279ddb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830632"
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a>Validação com a Interface IDataErrorInfo (c#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther mostra como exibir mensagens de erro de validação personalizada, Implementando a interface IDataErrorInfo em uma classe de modelo.


O objetivo deste tutorial é explicar uma abordagem para executar a validação em um aplicativo ASP.NET MVC. Você aprenderá a evitar que alguém envie um formulário HTML sem fornecer valores para campos de formulário necessária. Neste tutorial, você aprenderá a executar a validação usando a interface IErrorDataInfo.

## <a name="assumptions"></a>Suposições

Neste tutorial, usarei o banco de dados MoviesDB e a tabela de banco de dados de filmes. Esta tabela tem as seguintes colunas:

<a id="0.5_table01"></a>


| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | int | False |
| Título | Nvarchar(100) | False |
| Diretor | Nvarchar(100) | False |
| DateReleased | DateTime | False |


Neste tutorial, posso usar o Microsoft Entity Framework para gerar minhas classes de modelo de banco de dados. A classe Movie gerada pelo Entity Framework é exibida na Figura 1.


[![A entidade de filme](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Figura 01**: entidade o filme ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))


> [!NOTE] 
> 
> Para saber mais sobre como usar o Entity Framework para gerar classes de modelo de banco de dados, consulte a que minha tutorial o direito de criar Classes de modelo com o Entity Framework.


## <a name="the-controller-class"></a>A classe do controlador

Usamos o controlador Home filmes de lista e criar novos filmes. O código para essa classe está contido na listagem 1.

**Listagem 1 - Controllers\HomeController.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

A classe de controlador Home na listagem 1 contém duas ações Create (). A primeira ação exibe o formulário HTML para a criação de um novo filme. A segunda ação Create () executa a inserção real do novo filme no banco de dados. A segunda ação Create () é invocada quando o formulário exibido pela primeira ação Create () é enviado ao servidor.

Observe que a segunda ação Create () contém as seguintes linhas de código:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

A propriedade IsValid retorna false quando há um erro de validação. Nesse caso, o modo de exibição de criação que contém o formulário HTML para a criação de um filme é exibida novamente.

## <a name="creating-a-partial-class"></a>Criar uma classe parcial

A classe de filme é gerada pelo Entity Framework. Você pode ver o código para a classe de filme, se você expandir o arquivo MoviesDBModel.edmx na janela do Gerenciador de soluções e abra o arquivo de MoviesDBModel.Designer.cs no Editor de códigos (veja a Figura 2).


[![O código para a entidade de filme](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Figura 02**: O código para a entidade de filme ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))


A classe de filme é uma classe parcial. Isso significa que podemos adicionar outra classe parcial com o mesmo nome para estender a funcionalidade da classe filme. Vamos adicionar nossa lógica de validação para a nova classe parcial.

Adicione a classe na listagem 2 para a pasta de modelos.

**Listagem 2 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Observe que a classe na listagem 2 inclui o *parcial* modificador. Todos os métodos ou propriedades que você adiciona a essa classe se tornam parte da classe Movie gerada pelo Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Adicionando OnChanging e métodos de alguns OnChanged parcial

Quando o Entity Framework gera uma classe de entidade, o Entity Framework adiciona métodos parciais para a classe automaticamente. O Entity Framework gera OnChanging e alguns OnChanged métodos parciais que correspondem a cada propriedade da classe.

No caso da classe de filme, o Entity Framework cria os seguintes métodos:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

O método OnChanging é chamado correto antes da propriedade correspondente é alterada. O método de alguns OnChanged é chamado direita depois que a propriedade é alterada.

Você pode tirar proveito desses métodos parciais para adicionar lógica de validação para a classe de filme. A classe de filme na listagem 3 de atualização verifica que as propriedades Title e diretor recebem valores não vazios.

> [!NOTE] 
> 
> Um método parcial é um método definido em uma classe que não são necessários para implementar. Se você não implementar um método parcial, em seguida, o compilador removerá a assinatura do método e todas as chamadas para o método, então, aqui estão sem custos de tempo de execução associados ao método parcial. No código de Editor do Visual Studio, você pode adicionar um método parcial, digitando a palavra-chave *parcial* seguido por um espaço para exibir uma lista de parciais para implementar.


**Listagem 3 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Por exemplo, se você tentar atribuir uma cadeia de caracteres vazia para a propriedade de título, em seguida, uma mensagem de erro é atribuída a um dicionário chamado \_erros.

Neste ponto, nada realmente acontece quando você atribuir uma cadeia de caracteres vazia para a propriedade de título e um erro é adicionado ao particular \_campo de erros. É necessário implementar a interface IDataErrorInfo para expor esses erros de validação para a estrutura ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementando a Interface IDataErrorInfo

A interface IDataErrorInfo tem sido parte do .NET framework desde a primeira versão. Esta é uma interface muito simple:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Se uma classe implementa a interface IDataErrorInfo, o ASP.NET MVC framework usará essa interface ao criar uma instância da classe. Por exemplo, o controlador Home ação Create () aceita uma instância da classe filme:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

O ASP.NET MVC framework cria a instância do filme passado para a ação Create () usando um associador de modelo (o DefaultModelBinder). O associador de modelo é responsável por criar uma instância do objeto de filme, associando os campos de formulário HTML a uma instância do objeto de filme.

O DefaultModelBinder detecta se uma classe implementa a interface IDataErrorInfo. Se uma classe implementa essa interface, em seguida, o associador de modelo invoca o indexador IDataErrorInfo.this para cada propriedade da classe. Se o indexador retorna que uma mensagem de erro, em seguida, o associador de modelos adiciona essa mensagem de erro para modelar o estado automaticamente.

O DefaultModelBinder também verifica a propriedade IDataErrorInfo.Error. Esta propriedade destina-se para representar erros de validação específica da propriedade não associados à classe. Por exemplo, você talvez queira aplicar uma regra de validação que depende dos valores de várias propriedades da classe filme. Nesse caso, retornaria um erro de validação da propriedade de erro.

A classe Movie atualizada na listagem 4 implementa a interface IDataErrorInfo.

**Listagem 4 - Models\Movie.cs (implementa IDataErrorInfo)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

Na listagem 4, verifica a propriedade do indexador a \_coleção de erros para ver se ele contém uma chave que corresponde ao nome da propriedade é passado para o indexador. Se não houver nenhum erro de validação associado à propriedade, uma cadeia de caracteres vazia será retornada.

Você não precisa modificar o controlador inicial de qualquer forma ao usar a classe Movie modificada. A página exibida na Figura 3 ilustra o que acontece quando nenhum valor for inserido para os campos de título ou diretor do formulário.


[![Criação automática de métodos de ação](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Figura 03**: um formulário com valores ausentes ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))


Observe que o valor de DateReleased é validada automaticamente. Porque a propriedade DateReleased não aceita valores NULL, o DefaultModelBinder gera um erro de validação para essa propriedade automaticamente quando ele não tem um valor. Se você quiser modificar a mensagem de erro para a propriedade DateReleased, em seguida, você precisa criar um associador de modelo personalizado.

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu como usar a interface IDataErrorInfo para gerar mensagens de erro de validação. Primeiro, criamos uma classe parcial do filme que estende a funcionalidade da classe parcial do filme gerada pelo Entity Framework. Em seguida, adicionamos lógica de validação ao filme métodos da classe OnTitleChanging() e OnDirectorChanging() parciais. Por fim, implementamos a interface IDataErrorInfo para expor essas mensagens de validação para a estrutura MVC do ASP.NET.

> [!div class="step-by-step"]
> [Anterior](performing-simple-validation-cs.md)
> [Próximo](validating-with-a-service-layer-cs.md)
