---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: Usando anotações de dados para validação de modelo | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 6 aborda o uso de anotações de dados para o modelo V...
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 10884f569f0f5d95517b73daab31fbd269a4726a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825947"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Parte 6: Usando Data Annotations para validação de modelo
====================
por [Jon Galloway](https://github.com/jongalloway)

> A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.  
>   
> A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 6 aborda o uso de anotações de dados para a validação de modelo.


Temos um grande problema com nossos formulários de criar e editar: eles não estão fazendo nenhuma validação. Podemos fazer coisas como deixar os campos obrigatórios em branco ou tipo letras no campo de preço e o primeiro erro que veremos é do banco de dados.

Podemos facilmente adicionar validação ao nosso aplicativo com a adição de anotações de dados para nossas classes de modelo. Anotações de dados nos permitem descrever as regras que queremos que sejam aplicadas a nossas propriedades de modelo e o ASP.NET MVC se encarregará de impô-las e exibir as mensagens apropriadas para nossos usuários.

## <a name="adding-validation-to-our-album-forms"></a>Adicionando validação a nossos formulários de álbum

Vamos usar os seguintes atributos de anotação de dados:

- **Necessário** – indica que a propriedade é um campo obrigatório
- **DisplayName** – define o texto que desejamos usados em campos de formulário e mensagens de validação
- **StringLength** – define um comprimento máximo para um campo de cadeia de caracteres
- **Intervalo** – fornece um valor mínimo e máximo para um campo numérico
- **Associar** – lista de campos para excluir ou incluir ao associar os valores de parâmetro ou o formulário a propriedades de modelo
- **ScaffoldColumn** – permite ocultar campos de formulários do editor

*Observação: Para obter mais informações sobre validação de modelo usando atributos de anotação de dados, consulte a documentação do MSDN em*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Abra a classe de álbum e adicione o seguinte *usando* instruções na parte superior.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Em seguida, atualize as propriedades para adicionar atributos de validação e exibição, conforme mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Enquanto estamos fazendo isso, nós também alteramos o gênero e o artista para propriedades virtuais. Isso permite que o Entity Framework carregamento lento-los conforme necessário.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Depois de ter adicionado a esses atributos ao nosso modelo de álbum, nossa tela de criar e editar imediatamente começa a validar campos e usando os nomes de exibição, escolhemos (por exemplo, álbum arte Url em vez de AlbumArtUrl). Execute o aplicativo e navegue até /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Em seguida, dividiremos algumas regras de validação. Insira um preço de 0 e deixe o título em branco. Quando clicamos no botão Criar, vemos o formulário exibido com mensagens de erro de validação que mostra quais campos não atendeu as regras de validação definimos.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Testando a validação do lado do cliente

Validação do lado do servidor é muito importante da perspectiva do aplicativo, porque os usuários podem ignorar a validação do lado do cliente. No entanto, os formulários de página da Web que implementam a validação do lado do servidor só exibem três problemas consideráveis.

1. O usuário precisa esperar para o formulário seja postado, validados no servidor e para a resposta seja enviada ao seu navegador.
2. O usuário não obtém comentários imediatos quando eles corrigem um campo para que ele agora passa as regras de validação.
3. Nós são desperdício de recursos de servidor para executar a lógica de validação em vez de aproveitar o navegador do usuário.

Felizmente, os modelos do scaffold ASP.NET MVC 3 tem validação do lado do cliente interna, não exigindo nenhum trabalho adicional para qualquer tipo.

Digitar uma única letra no campo título satisfaz os requisitos de validação, para que a mensagem de validação é removida imediatamente.

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-5.md)
> [Próximo](mvc-music-store-part-7.md)
