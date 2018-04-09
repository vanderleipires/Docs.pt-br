---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Criar um cliente JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 29d50e448e6d282c7db06b9d1946ac221347e1ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="create-the-javascript-client"></a>Criar um cliente JavaScript
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você criará o cliente para o aplicativo usando HTML, JavaScript e o [Knockout. js](http://knockoutjs.com/) biblioteca. Criaremos o aplicativo cliente em estágios:

- Mostrando uma lista de livros.
- Mostrando um detalhe de catálogo.
- Adicionando um novo catálogo.

A biblioteca de separação usa o padrão Model-View-ViewModel (MVVM):

- O **modelo** é a representação do lado do servidor dos dados no domínio da empresa (no nosso caso, manuais e autores).
- O **exibição** é a camada de apresentação (HTML).
- O **modelo de exibição** é um objeto JavaScript que contém os modelos. O modelo de exibição é uma abstração de código da interface do usuário. Ele não tem conhecimento da representação de HTML. Em vez disso, ele representa recursos abstratos da exibição, como &quot;uma lista de livros&quot;.

O modo de exibição associado a dados para o modelo de exibição. Atualizações para o modelo de exibição são refletidas automaticamente no modo de exibição. O modelo de exibição também obtém os eventos da exibição, como cliques de botão.

![](part-6/_static/image1.png)

Essa abordagem facilita a alterar o layout e a interface do usuário do seu aplicativo, porque você pode alterar as associações, sem reescrever qualquer código. Por exemplo, você pode mostrar uma lista de itens como um `<ul>`, altere-o posteriormente para uma tabela.

## <a name="add-the-knockout-library"></a>Adicionar a biblioteca de separação

No Visual Studio, do **ferramentas** menu, selecione **Gerenciador de pacotes de biblioteca**. Em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](part-6/samples/sample1.cmd)]

Este comando adiciona os arquivos de separação para a pasta Scripts.

## <a name="create-the-view-model"></a>Criar o modelo de exibição

Adicione um arquivo JavaScript chamado app.js para a pasta Scripts. (No Gerenciador de soluções, clique na pasta Scripts, selecione **adicionar**, em seguida, selecione **arquivo JavaScript**.) Cole o código a seguir:

[!code-javascript[Main](part-6/samples/sample2.js)]

Na separação, a `observable` classe permite que a associação de dados. Quando altera o conteúdo do observável, o observável notifica todos os controles de associação de dados, para que possa atualizar a mesmos. (O `observableArray` classe é a versão de matriz de *observável*.) Para iniciar com nosso modelo de exibição tem dois observáveis:

- `books` contém a lista de livros.
- `error` contém uma mensagem de erro se uma chamada AJAX falhar.

O `getAllBooks` método faz uma chamada AJAX para obter a lista de livros. Em seguida, ele envia o resultado para o `books` matriz.

O `ko.applyBindings` método é parte da biblioteca de separação. Ele usa o modelo de exibição como um parâmetro e configura a associação de dados.

## <a name="add-a-script-bundle"></a>Adicionar um pacote de Script

Agrupamento é um recurso no ASP.NET 4.5 que torna mais fácil combinar ou agrupar vários arquivos em um único arquivo. Agrupando reduz o número de solicitações para o servidor, o que pode melhorar o tempo de carregamento de página.

Abra o arquivo de aplicativo\_Start/BundleConfig.cs. Adicione o seguinte código ao método RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Anterior](part-5.md)
> [Próximo](part-7.md)
