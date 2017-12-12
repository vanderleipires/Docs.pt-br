---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: "Iteração #2 – tornar o aplicativo parecer adequado (c#) | Microsoft Docs"
author: microsoft
description: "Essa iteração, podemos melhorar a aparência do aplicativo, modificando a página mestra do ASP.NET MVC exibição padrão e em cascata a folha de estilos."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: 10379f5321773155aaff4c384d8e0716d7e0e874
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="iteration-2--make-the-application-look-nice-c"></a>Iteração #2 – tornar o aplicativo parecer adequado (c#)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> Essa iteração, podemos melhorar a aparência do aplicativo, modificando a página mestra do ASP.NET MVC exibição padrão e em cascata a folha de estilos.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Criando um aplicativo ASP.NET MVC de gerenciamento de contatos (c#)
  

Esta série de tutoriais, vamos criar um aplicativo de gerenciamento de entre em contato com toda a partir do início ao fim. O aplicativo Gerenciador de entrar em contato com permite armazenar informações de contato - nomes, números de telefone e endereços de email - para obter uma lista de pessoas.

Vamos criar o aplicativo em várias iterações. Com cada iteração, podemos melhorar gradualmente o aplicativo. O objetivo dessa abordagem iteração vários é para que você possa entender o motivo para cada alteração.

- Iteração #1 - criar o aplicativo. A primeira iteração, criamos o gerente do contato da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básicos: criar, leitura, atualização e exclusão (CRUD).

- Iteração #2 - Verifique o aplicativo parecer adequado. Essa iteração, podemos melhorar a aparência do aplicativo, modificando a página mestra do ASP.NET MVC exibição padrão e em cascata a folha de estilos.

- Iteração #3 - adicionar validação do formulário. Na terceira iteração, podemos adicionar validação de forma básica. Vamos impedir que pessoas enviando um formulário sem concluir os campos obrigatórios do formulário. Além disso, podemos validar endereços de email e números de telefone.

- Iteração #4 - Verifique o aplicativo acoplados de forma flexível. Essa terceira iteração, podemos aproveitar vários padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contato. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- Iteração #5 - criar testes de unidade. Na quinta iteração, fazemos nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular nossas classes de modelo de dados e criar testes de unidade para nossos controladores e lógica de validação.

- Iteração #6 - Use desenvolvimento controlado por testes. Essa iteração do sexto, adicionamos novas funcionalidades para nosso aplicativo escrevendo testes de unidade primeiro e escrever código os testes de unidade. Essa iteração, adicionamos grupos de contatos.

- Iteração #7 - adicionar funcionalidade Ajax. A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.

## <a name="this-iteration"></a>Essa iteração

O objetivo dessa iteração é melhorar a aparência do aplicativo Gerenciador de contato. Atualmente, o gerente do contato usa a página mestra do ASP.NET MVC exibição padrão e a folha de estilos em cascata (consulte a Figura 1). Esses don t examinar incorreta, mas não quiser t o gerente do contato assim como todos os outros sites ASP.NET MVC. Desejo substituir esses arquivos com arquivos personalizados.


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Figura 01**: A aparência padrão de um aplicativo ASP.NET MVC ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image2.png))


Essa iteração, falarei sobre duas abordagens para melhorar o design visual de nosso aplicativo. Primeiro, mostrarei como tirar proveito da Galeria de Design do ASP.NET MVC para baixar um modelo de projeto ASP.NET MVC livre. A Galeria de Design do ASP.NET MVC permite que você crie um aplicativo web professional sem fazer qualquer trabalho.

Decidi usar um modelo da Galeria de Design do ASP.NET MVC para o aplicativo do Gerenciador de contato. Em vez disso, tinha um design personalizado criado por uma empresa de design profissional. A segunda parte deste tutorial, explicarei como trabalhei com uma empresa de design profissional para criar o design final do ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>A Galeria de Design do ASP.NET MVC

A Galeria de Design do ASP.NET MVC é um recurso gratuito fornecido pela Microsoft. A Galeria do ASP.NET MVC está localizada no seguinte endereço:

[https://www.ASP.NET/MVC/Gallery](https://www.asp.net/mvc/gallery)

A Galeria de Design do ASP.NET MVC hospeda uma coleção de designs de site gratuito que foram criados especificamente para uso em um projeto ASP.NET MVC. Os designs são carregados por membros da comunidade. Os visitantes a galeria podem votar em seus favoritos designs (consulte a Figura 2).


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Figura 02**: A Galeria de Design do ASP.NET MVC ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image4.png))


Como posso gravar este tutorial, o design mais popular na Galeria de é um projeto chamado outubro por David Hauser. Você pode usar esse design de um projeto ASP.NET MVC completando as etapas a seguir:

1. Clique o **baixar** botão para baixar o arquivo October.zip para seu computador.
2. Clique com botão direito no arquivo October.zip baixado e clique no **desbloquear** botão (consulte a Figura 3).
3. Descompacte o arquivo para uma pasta chamada outubro.
4. Selecionar todos os arquivos da pasta DesignTemplate contida na pasta de outubro, os arquivos de atalho e selecione a opção de menu **cópia**.
5. Clique com botão direito no nó do projeto ContactManager na janela do Gerenciador de soluções do Visual Studio e selecione a opção de menu **colar** (consulte a Figura 4).
6. Selecione a opção de menu do Visual Studio **editar, localizar e substituir, substituição rápida** e substitua *[MyProjectName]* com *ContactManager* (consulte a Figura 5).


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Figura 03**: desbloquear um arquivo baixado da web ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image6.png))


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Figura 04**: substituindo arquivos no Gerenciador de soluções ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image8.png))


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Figura 05**: substituindo [ProjectName] com ContactManager ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image10.png))


Depois de concluir essas etapas, seu aplicativo da web usará o novo design. A página na Figura 6 ilustra a aparência do aplicativo Gerenciador de contato com o design de outubro.


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Figura 06**: ContactManager com o modelo de outubro ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Criando um Design MVC do ASP.NET personalizadas

A Galeria de Design do ASP.NET MVC tem uma boa seleção de estilos de design diferentes. A Galeria fornece uma maneira simples de personalizar a aparência de seus aplicativos ASP.NET MVC. E, claro, a galeria tem a grande vantagem de estar totalmente livre.

No entanto, você precisará criar um design completamente exclusivo para seu site. Nesse caso, faz sentido para trabalhar com uma empresa de design do site. Decidi essa abordagem para o design do aplicativo do gerente do contato.

Eu compactado o Gerenciador de contato de iteração #1 e enviadas do projeto para a empresa de design. Eles não possuía Visual Studio (pena neles!), mas que não foi apresentar um problema. Ele conseguiu baixar o Microsoft Visual Web Developer livre do [https://www.asp.net](https://www.asp.net) site e abra o aplicativo Gerenciador de contato no Visual Web Developer. Em alguns dias, eles tinham gerados design na Figura 7.


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Figura 07**: O projeto ASP.NET MVC Contact Manager ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image14.png))


O novo design consistiu em dois arquivos principais: um novo arquivo de folha de estilo em cascata e um novo arquivo página mestre de modo de exibição. Uma página de exibição mestre contém o layout e o conteúdo compartilhado para modos de exibição em um aplicativo ASP.NET MVC. Por exemplo, a página mestra do modo de exibição inclui o cabeçalho, guias de navegação e rodapé aparecem na Figura 7. Substituiu o a página de mestre de modo de exibição existente do Site.Master na pasta exibições \ compartilhadas com o novo arquivo Site.Master da empresa de design,

Além disso, a empresa de design criada uma nova folha de estilos em cascata e um conjunto de imagens. Eu colocado novos arquivos na pasta de conteúdo e substituiu o arquivo de site existente. Você deve colocar todo o conteúdo estático na pasta de conteúdo.

Observe que o novo design para o gerente do contato inclui imagens para editar e excluir contatos. Uma imagem de editar e excluir aparecer ao lado de cada contato na tabela HTML de contatos.

Originalmente, esses links que foram processados com o HTML. Auxiliar do ActionLink() como este:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

O método Html.ActionLink() não oferece suporte a imagens (o método HTML codifica o texto do link por motivos de segurança). Portanto, eu substituído as chamadas para Html.ActionLink() com chamadas para Url.Action() como este:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

O método Html.ActionLink() renderiza um hiperlink HTML inteiro. O método Url.Action(), por outro lado, processa apenas a URL sem o &lt;um&gt; marca.

Além disso, observe que o novo design inclui guias selecionadas e desmarcadas. Por exemplo, na Figura 8, o **criar novo contato** guia é selecionada e o **Meus contatos** guia não está selecionada.


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Figura 08**: marcadas e desmarcadas guias ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image16.png))


Para dar suporte a renderização guias selecionadas e desmarcadas, criei um auxiliar HTML personalizado chamado o MenuItemHelper. Esse método auxiliar é renderizado em um &lt;li&gt; marca ou um &lt;classe li = "selecionado"&gt; marca dependendo se o controlador atual e a ação corresponde ao nome do controlador e ação passado para o auxiliar. O código para o MenuItemHelper está contido na listagem 1.

**Listando 1 - Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

O MenuItemHelper usa a classe TagBuilder internamente para criar o &lt;li&gt; tag HTML. A classe TagBuilder é uma classe de utilitário muito útil que você pode usar sempre que você precisa criar uma nova marca HTML. Ele inclui métodos para adicionar atributos, adicionando classes CSS, gerar Ids e modificando a marca s HTML interno.

## <a name="summary"></a>Resumo

Essa iteração, melhoramos o design visual de nosso aplicativo ASP.NET MVC. Primeiro, você foram introduzidos para a Galeria de Design do ASP.NET MVC. Você aprendeu como baixar os modelos de projeto gratuito da Galeria de Design ASP.NET MVC que você pode usar em seus aplicativos ASP.NET MVC.

Em seguida, discutimos como você pode criar um design personalizado modificando o arquivo de folha de estilo em cascata padrão e o arquivo de paginação de modo de exibição mestre. Para dar suporte ao novo design, tivemos que fazer algumas pequenas alterações para nosso aplicativo Gerenciador de contato. Por exemplo, adicionamos um novo auxiliar HTML chamado o MenuItemHelper que exibe as guias selecionadas e desmarcadas.

Na próxima iteração, vamos lidar com o assunto muito importante da validação. Vamos adicionar código de validação para o nosso aplicativo para que um usuário não é possível criar um novo contato sem fornecer valores necessários, como uma pessoa s primeiro e último nome.

>[!div class="step-by-step"]
[Anterior](iteration-1-create-the-application-cs.md)
[Próximo](iteration-3-add-form-validation-cs.md)
