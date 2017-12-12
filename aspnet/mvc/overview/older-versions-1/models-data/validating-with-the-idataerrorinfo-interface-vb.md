---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Validando com a Interface IDataErrorInfo (VB) | Microsoft Docs
author: StephenWalther
description: "Stephen Walther mostra como exibir mensagens de erro de validação personalizado implementando a interface IDataErrorInfo em uma classe de modelo."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 1439d470a7fa3cb1171dbdd0b7eec6a6aa52912d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a>Validando com a Interface IDataErrorInfo (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther mostra como exibir mensagens de erro de validação personalizado implementando a interface IDataErrorInfo em uma classe de modelo.


O objetivo deste tutorial é explicar um método para executar a validação em um aplicativo ASP.NET MVC. Você aprenderá a impedir que alguém enviar um formulário HTML sem fornecer valores para os campos obrigatórios do formulário. Neste tutorial, você aprenderá a executar a validação usando a interface IErrorDataInfo.

## <a name="assumptions"></a>Suposições

Neste tutorial, usarei o banco de dados MoviesDB e a tabela de banco de dados de filmes. Esta tabela tem as seguintes colunas:

<a id="0.6_table01"></a>


| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | int | False |
| Título | nvarchar (100) | False |
| Diretor | nvarchar (100) | False |
| DateReleased | DateTime | False |


Neste tutorial, posso usar o Microsoft Entity Framework para gerar classes de modelo meu banco de dados. A classe de filme gerada pelo Entity Framework é exibida na Figura 1.


[![A entidade de filme](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

**Figura 01**: entidade o filme ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))


> [!NOTE] 
> 
> Para saber mais sobre como usar o Entity Framework para gerar as classes de modelo de banco de dados, consulte o tutorial Criando Classes de modelo com o Entity Framework.


## <a name="the-controller-class"></a>A classe do controlador

Podemos usar o controlador Home filmes de lista e criar novos filmes. O código para essa classe está contido na listagem 1.

**Listando 1 - Controllers\HomeController.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

A classe do controlador Home na listagem 1 contém duas ações Create (). A primeira ação exibe o formulário HTML para criar um novo filme. A segunda ação Create () executa a inserção real do novo filme no banco de dados. A segunda ação Create () é chamada quando o formulário exibido, a primeira ação Create () é enviado ao servidor.

Observe que a segunda ação Create () contém linhas de código a seguir:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

A propriedade IsValid retorna false quando há um erro de validação. Nesse caso, o modo de exibição de criação que contém o formulário HTML para a criação de um filme é exibida novamente.

## <a name="creating-a-partial-class"></a>Criar uma classe parcial

A classe de filme é gerada pelo Entity Framework. Você pode ver o código para a classe de filme, se você expandir o arquivo MoviesDBModel.edmx na janela do Gerenciador de soluções e abra o arquivo MoviesDBModel.Designer.vb no Editor de código (consulte a Figura 2).


[![O código para a entidade de filme](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

**Figura 02**: O código para a entidade de filme ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))


A classe de filme é uma classe parcial. Isso significa que podemos adicionar outra classe parcial com o mesmo nome para estender a funcionalidade da classe filme. Vamos adicionar nossa lógica de validação para a nova classe parcial.

Adicione a classe na lista 2 para a pasta de modelos.

**A listagem 2 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

Observe que a classe na lista 2 inclui a *parcial* modificador. Quaisquer métodos ou propriedades que você adicionar a essa classe se tornam parte da classe de filme gerado pelo Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Adicionando OnChanging e métodos OnChanged Partial

Quando o Entity Framework gera uma classe de entidade, o Entity Framework adiciona métodos parciais para a classe automaticamente. O Entity Framework gera OnChanging e OnChanged métodos parciais que correspondem a cada propriedade da classe.

No caso da classe de filme, o Entity Framework cria os seguintes métodos:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

O método OnChanging é chamado correto antes da propriedade correspondente é alterada. O método OnChanged é chamado direita depois que a propriedade é alterada.

Você pode tirar proveito desses métodos parciais para adicionar lógica de validação para a classe do filme. A classe de filme na listagem 3 de atualização verifica que as propriedades Title e diretor são atribuídas valores não vazios.

> [!NOTE] 
> 
> Um método parcial é um método definido em uma classe que não é necessário para implementar. Se você não implementa um método parcial, em seguida, o compilador remove a assinatura do método e todas as chamadas para o método para que estão sem qualquer custo de tempo de execução associado com o método parcial. No código de Editor do Visual Studio, você pode adicionar um método parcial, digitando a palavra-chave *parcial* seguido por um espaço para exibir uma lista de existe meio-termo para implementar.


**A listagem 3 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

Por exemplo, se você tentar atribuir uma cadeia de caracteres vazia para a propriedade Title, em seguida, uma mensagem de erro é atribuída a um dicionário, chamado \_erros.

Neste ponto, nada acontece mesmo quando você atribui uma cadeia de caracteres vazia para a propriedade de título e um erro será adicionado à particular \_campo de erros. É necessário implementar a interface IDataErrorInfo para expor esses erros de validação para a estrutura ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementando a Interface IDataErrorInfo

A interface IDataErrorInfo foi parte do .NET framework desde a primeira versão. Esta é uma interface muito simple:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

Se uma classe implementa a interface IDataErrorInfo, a estrutura ASP.NET MVC usará essa interface ao criar uma instância da classe. Por exemplo, o controlador Home ação Create () aceita uma instância da classe filme:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

A estrutura ASP.NET MVC cria a instância do filme passado para a ação Create () usando um associador de modelo (o DefaultModelBinder). O associador de modelo é responsável pela criação de uma instância do objeto do filme ao associar os campos de formulário HTML para uma instância do objeto de filme.

O DefaultModelBinder detecta se uma classe implementa a interface IDataErrorInfo ou não. Se uma classe implementa essa interface o associador de modelo invoca o indexador IDataErrorInfo.this para cada propriedade da classe. Se o indexador retorna uma mensagem de erro o associador de modelo adiciona essa mensagem de erro para modelar o estado automaticamente.

O DefaultModelBinder também verifica a propriedade IDataErrorInfo.Error. Essa propriedade é serve para representar os erros de validação específicos de propriedade não associados à classe. Por exemplo, você talvez queira aplicar uma regra de validação que depende dos valores de várias propriedades da classe filme. Nesse caso, você retornará um erro de validação da propriedade de erro.

A classe de filme atualizada na listagem 4 implementa a interface IDataErrorInfo.

**A listagem 4 - Models\Movie.vb (implementa IDataErrorInfo)**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

Na listagem 4, verifica a propriedade do indexador a \_coleção de erros para ver se ele contém uma chave que corresponde ao nome da propriedade é passado para o indexador. Se não houver nenhum erro de validação associado com a propriedade é retornada uma cadeia de caracteres vazia.

Você não precisa modificar o controlador inicial de qualquer forma ao usar a classe de filme modificada. A página exibida na Figura 3 ilustra o que acontece quando nenhum valor for inserido para os campos de formulário título ou Director.


[![Criação automática de métodos de ação](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

**Figura 03**: um formulário com valores ausentes ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))


Observe que o valor de DateReleased é validada automaticamente. Porque a propriedade DateReleased não aceita valores NULL, o DefaultModelBinder gera um erro de validação para esta propriedade automaticamente quando ele não tem um valor. Se você quiser modificar a mensagem de erro para a propriedade DateReleased, em seguida, você precisa criar um associador de modelo personalizado.

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu a usar a interface IDataErrorInfo para gerar mensagens de erro de validação. Primeiro, criamos uma classe parcial de filme que estende a funcionalidade da classe filme parcial gerada pelo Entity Framework. Em seguida, adicionamos a lógica de validação para os filme classe OnTitleChanging() e OnDirectorChanging() métodos parciais. Por fim, implementamos a interface IDataErrorInfo para expor essas mensagens de validação para a estrutura ASP.NET MVC.

>[!div class="step-by-step"]
[Anterior](performing-simple-validation-vb.md)
[Próximo](validating-with-a-service-layer-vb.md)
