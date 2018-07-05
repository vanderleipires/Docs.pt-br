---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Criar o cliente JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b0b8ef9bd44bbce5102f2b12717e330f72a9e0c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400928"
---
<a name="create-the-javascript-client"></a>Criar o cliente JavaScript
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você irá criar o cliente para o aplicativo usando HTML, JavaScript e o [Knockout. js](http://knockoutjs.com/) biblioteca. Vamos criar o aplicativo cliente em estágios:

- Mostrando uma lista de livros.
- Mostrando detalhes de um livro.
- Adicionando um novo catálogo.

A biblioteca Knockout usa o padrão Model-View-ViewModel (MVVM):

- O **modelo** é a representação do lado do servidor dos dados no domínio de negócios (no nosso caso, manuais e autores).
- O **exibição** é a camada de apresentação (HTML).
- O **modelo de exibição** é um objeto JavaScript que contém os modelos. O modelo de exibição é uma abstração de código da interface do usuário. Ele não tem conhecimento da representação HTML. Em vez disso, ele representa recursos abstratos da exibição, como &quot;uma lista de livros&quot;.

O modo de exibição associado a dados para o modelo de exibição. Atualizações para o modelo de exibição são refletidas automaticamente no modo de exibição. O modelo de exibição também obtém os eventos da exibição, como cliques de botão.

![](part-6/_static/image1.png)

Essa abordagem facilita a alterar o layout e interface do usuário do seu aplicativo, como você pode alterar as associações, sem reescrever nenhum código. Por exemplo, você pode mostrar uma lista de itens como um `<ul>`, alterá-lo posteriormente a uma tabela.

## <a name="add-the-knockout-library"></a>Adicionar a biblioteca Knockout

No Visual Studio, do **ferramentas** menu, selecione **Library Package Manager**. Em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](part-6/samples/sample1.cmd)]

Este comando adiciona os arquivos do Knockout para a pasta Scripts.

## <a name="create-the-view-model"></a>Criar o modelo de exibição

Adicione um arquivo JavaScript chamado App. js para a pasta Scripts. (No Gerenciador de soluções, clique com botão direito na pasta de Scripts, selecione **Add**, em seguida, selecione **arquivo JavaScript**.) Cole o código a seguir:

[!code-javascript[Main](part-6/samples/sample2.js)]

No Knockout, a `observable` classe permite que a associação de dados. Quando altera o conteúdo de um observável, o observável notifica todos os controles ligados a dados, para que possam atualizar em si. (O `observableArray` classe é a versão de matriz de *observável*.) Para começar, nosso modelo de exibição tem dois observáveis:

- `books` contém a lista de livros.
- `error` contém uma mensagem de erro se uma chamada AJAX falhar.

O `getAllBooks` método faz uma chamada ao AJAX para obter a lista de livros. Em seguida, ele envia o resultado para o `books` matriz.

O `ko.applyBindings` método faz parte da biblioteca do Knockout. Ele usa o modelo de exibição como um parâmetro e configura a vinculação de dados.

## <a name="add-a-script-bundle"></a>Adicionar um pacote de Script

O agrupamento é um recurso do ASP.NET 4.5 que torna mais fácil combinar ou agrupar vários arquivos em um único arquivo. Agrupamento reduz o número de solicitações para o servidor, o que pode melhorar o tempo de carregamento de página.

Abra o arquivo de aplicativo\_Start/BundleConfig.cs. Adicione o seguinte código ao método RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Anterior](part-5.md)
> [Próximo](part-7.md)
