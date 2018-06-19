---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: Usando as anotações de dados para validação de modelo | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 6 abrange usando as anotações de dados modelo V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 328eccb4324bb10a7e8dec819a70129fc14c42c4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872410"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Parte 6: Usando as anotações de dados para a validação de modelo
====================
por [Jon Galloway](https://github.com/jongalloway)

> O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.  
>   
> O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 6 abrange o uso de anotações de dados para a validação do modelo.


Temos um grande problema com nosso formas de criar e editar: não fazem nenhuma validação. Podemos fazer coisas como deixar em branco os campos obrigatórios ou letras de tipo no campo de, e é o primeiro erro que veremos do banco de dados.

Podemos facilmente adicionar validação para o nosso aplicativo adicionando anotações de dados para nosso classes de modelo. As anotações de dados que descrevem as regras que queremos aplicadas às nossas propriedades de modelo e ASP.NET MVC cuidará de aplicá-las e exibir as mensagens apropriadas para os usuários.

## <a name="adding-validation-to-our-album-forms"></a>Adicionando validação a nossos formulários álbum

Vamos usar os seguintes atributos de anotação de dados:

- **Necessário** – indica que a propriedade é um campo obrigatório
- **DisplayName** – define o texto que desejamos usados nos campos de formulário e mensagens de validação
- **StringLength** – define um comprimento máximo de um campo de cadeia de caracteres
- **Intervalo** – fornece um valor mínimo e máximo para um campo numérico
- **Associar** – lista de campos para excluir ou incluir ao associar valores de parâmetro ou formulário a propriedades de modelo
- **ScaffoldColumn** – permite ocultar campos de formulários do editor

*Observação: Para obter mais informações sobre a validação de modelo usando atributos de anotação de dados, consulte a documentação do MSDN em*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Abra a classe álbum e adicione o seguinte *usando* instruções para a parte superior.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Em seguida, atualize as propriedades para adicionar atributos de exibição e a validação, conforme mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Enquanto estiver lá, nós também alteramos o gênero e artista para propriedades virtuais. Isso permite que o Entity Framework lento-carga-los conforme necessário.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Após ter adicionado a esses atributos ao nosso modelo álbum, nossa tela criar e editar imediatamente começa a validar campos e usando os nomes de exibição que escolhemos (por exemplo, álbum de arte Url em vez de AlbumArtUrl). Execute o aplicativo e navegue até /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Em seguida, vamos quebrar algumas regras de validação. Insira um preço de 0 e deixe o título em branco. Quando é clicar no botão Criar, vemos o formulário exibido com mensagens de erro de validação mostrando os campos que não atendeu as regras de validação definimos.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Testando a validação do lado do cliente

Validação do lado do servidor é muito importante da perspectiva do aplicativo, porque os usuários podem contornar a validação do lado do cliente. No entanto, os formulários de página da Web que implementam a validação do lado do servidor só exibem três problemas significativos.

1. O usuário deve aguardar para o formulário a ser lançada, validados no servidor e para a resposta a ser enviada ao navegador.
2. O usuário não obter feedback imediato quando eles corrigir um campo para que ele seja aprovado agora as regras de validação.
3. Nós são desperdício de recursos de servidor para executar a lógica de validação em vez de utilizar o navegador do usuário.

Felizmente, os modelos de scaffold do ASP.NET MVC 3 têm validação do lado do cliente interna, exigindo sem trabalho adicional de qualquer tipo.

Digitar uma única letra no campo título satisfaz os requisitos de validação, portanto, a mensagem de validação é removida imediatamente.

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-5.md)
> [Próximo](mvc-music-store-part-7.md)
