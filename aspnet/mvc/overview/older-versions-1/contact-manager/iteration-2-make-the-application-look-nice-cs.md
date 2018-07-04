---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: 'Iteração #2 – tornar o aplicativo parecer interessante (c#) | Microsoft Docs'
author: microsoft
description: Nesta iteração, podemos melhorar a aparência do aplicativo modificando a página mestra do ASP.NET MVC exibição padrão e em cascata de folha de estilos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: e78383516c752748da67f058c37aeb66d7004707
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377922"
---
<a name="iteration-2--make-the-application-look-nice-c"></a>Iteração #2 – tornar o aplicativo parecer interessante (c#)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> Nesta iteração, podemos melhorar a aparência do aplicativo modificando a página mestra do ASP.NET MVC exibição padrão e em cascata de folha de estilos.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Criando um aplicativo ASP.NET MVC de gerenciamento de contatos (c#)
  

Esta série de tutoriais, vamos criar um aplicativo de gerenciamento de contatos inteiro do início ao fim. O aplicativo Gerenciador de contatos permite que você armazene informações de contato - nomes, números de telefone e endereços de email - para obter uma lista de pessoas.

Criamos o aplicativo ao longo de várias iterações. Com cada iteração, podemos melhorar gradualmente o aplicativo. A meta dessa abordagem de iteração vários é que você possa entender o motivo para cada alteração.

- Iteração #1 - criar o aplicativo. A primeira iteração, podemos criar o Gerenciador de contatos da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básico: criar, ler, atualizar e excluir (CRUD).

- Iteração #2 - tornar o aplicativo interessante. Nesta iteração, podemos melhorar a aparência do aplicativo modificando a página mestra do ASP.NET MVC exibição padrão e em cascata de folha de estilos.

- Iteração #3 - adicionar validação de formulário. Na terceira iteração, podemos adicionar validação de formulário básico. Podemos impedir que pessoas enviando um formulário sem preencher os campos obrigatórios do formulário. Podemos também validar endereços de email e números de telefone.

- Iteração #4 – tornar o aplicativo fracamente acoplado. Nesta terceira iteração, podemos tirar proveito dos diversos padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contatos. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- Iteração #5 - criar testes de unidade. Na quinta iteração, podemos tornar nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular a nossas classes de modelo de dados e criar testes de unidade para nossos controladores e lógica de validação.

- Iteração #6 – usar desenvolvimento controlado por teste. Essa iteração sexta, adicionamos novas funcionalidades ao nosso aplicativo escrevendo testes de unidade pela primeira vez e escrever código contra os testes de unidade. Essa iteração, adicionamos os grupos de contatos.

- Iteração #7 - adicionar a funcionalidade do Ajax. A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.

## <a name="this-iteration"></a>Essa iteração

O objetivo dessa iteração é melhorar a aparência do aplicativo Gerenciador de contatos. Atualmente, o Gerenciador de contatos usa a página mestre de modo de exibição ASP.NET MVC padrão e a folha de estilos em cascata (consulte a Figura 1). Esses don t parecem ruins, mas não deseja t o Gerenciador de contato são semelhante a todos os outros sites de ASP.NET MVC. Eu quiser substituir esses arquivos com arquivos personalizados.


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Figura 01**: A aparência padrão de um aplicativo ASP.NET MVC ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image2.png))


Essa iteração, falarei sobre duas abordagens para aprimorar o design visual do nosso aplicativo. Primeiro, mostrarei como tirar proveito da Galeria de Design do ASP.NET MVC para baixar um modelo de design livre do ASP.NET MVC. A Galeria de Design do ASP.NET MVC permite que você crie um aplicativo web profissional sem fazer qualquer trabalho.

Decidi usar um modelo da Galeria de Design do ASP.NET MVC para o aplicativo Gerenciador de contatos. Em vez disso, eu tinha um design personalizado criado por uma empresa de design profissional. Na segunda parte deste tutorial, explicarei como eu trabalhei com uma empresa de design profissional para criar o design final do ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>A Galeria de Design do ASP.NET MVC

A Galeria de Design do ASP.NET MVC é um recurso gratuito fornecido pela Microsoft. A Galeria de MVC do ASP.NET está localizada no seguinte endereço:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

A Galeria de Design do ASP.NET MVC hospeda uma coleção de projetos de site gratuito que foram criados especificamente para usar em um projeto ASP.NET MVC. Designs são carregados por membros da comunidade. Os visitantes à Galeria podem votar para seus designs de Favoritos (consulte a Figura 2).


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Figura 02**: A Galeria de Design do ASP.NET MVC ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image4.png))


Enquanto escrevo este tutorial, o design mais popular na Galeria é um design denominado outubro por David Hauser. Você pode usar esse design para um projeto ASP.NET MVC, completando as etapas a seguir:

1. Clique o **baixar** botão para baixar o arquivo October.zip em seu computador.
2. O arquivo October.zip baixado com o botão direito e clique no **Unblock** botão (consulte a Figura 3).
3. Descompacte o arquivo para uma pasta chamada outubro.
4. Selecionar todos os arquivos da pasta DesignTemplate contida na pasta de outubro, os arquivos com o botão direito e selecione a opção de menu **cópia**.
5. Clique com botão direito no nó do projeto ContactManager na janela do Gerenciador de soluções do Visual Studio e selecione a opção de menu **colar** (veja a Figura 4).
6. Selecione a opção de menu do Visual Studio **editar, localizar e substituir, substituição rápida** e substitua *[MyProjectName]* com *ContactManager* (consulte a Figura 5).


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Figura 03**: desbloquear um arquivo baixado da web ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image6.png))


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Figura 04**: substituição de arquivos no Gerenciador de soluções ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image8.png))


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Figura 05**: substituindo [ProjectName] ContactManager ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image10.png))


Depois de concluir essas etapas, seu aplicativo da web usará o novo design. A página na Figura 6 ilustra a aparência do aplicativo Gerenciador de contatos com o design de outubro.


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Figura 06**: ContactManager com o modelo de outubro ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Criando um projeto de MVC do ASP.NET personalizados

A Galeria de Design do ASP.NET MVC tem uma boa seleção de estilos de design diferentes. A Galeria fornece uma maneira fácil de personalizar a aparência de seus aplicativos ASP.NET MVC. E, claro, a galeria tem a grande vantagem de ser totalmente gratuito.

No entanto, você talvez precise criar um design completamente exclusivo para seu site. Nesse caso, faz sentido para trabalhar com uma empresa de design do site. Decidi usar essa abordagem para o design para o aplicativo Gerenciador de contatos.

Eu compactado o Gerenciador de contato de iteração n º 1 e enviadas do projeto para a empresa de design. Eles não possuía Visual Studio (que horror neles!), mas que t apresentam um problema. Eles foram capazes de baixar o Microsoft Visual Web Developer gratuitamente do [ https://www.asp.net ](https://www.asp.net) site e abra o aplicativo Gerenciador de contatos no Visual Web Developer. Em alguns dias, eles tinham produzido o design na Figura 7.


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Figura 07**: O Design do Gerenciador de contatos do ASP.NET MVC ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image14.png))


O novo design consistiu em dois arquivos principais: um novo arquivo de folha de estilos em cascata e um novo modo de exibição mestre arquivo de paginação. Uma página mestra do modo de exibição contém o layout e o conteúdo compartilhado para modos de exibição em um aplicativo ASP.NET MVC. Por exemplo, a página mestra do modo de exibição inclui o cabeçalho, guias de navegação e rodapé aparecem na Figura 7. Substituiu o a página de mestre de modo de exibição existente do site na pasta Views\Shared com o novo arquivo de site da empresa de design,

A empresa de design também é criada uma nova folha de estilos em cascata e um conjunto de imagens. Eu colocado esses novos arquivos na pasta de conteúdo e substituiu o arquivo CSS existente. Você deve colocar todo o conteúdo estático na pasta de conteúdo.

Observe que o novo design para o Gerenciador de contatos inclui imagens para editar e excluir contatos. Uma imagem de edição e exclusão aparecer ao lado de cada contato na tabela HTML de contatos.

Originalmente, esses links que foram processados com o HTML. Auxiliar de ActionLink() como este:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

O método Html.ActionLink() não oferece suporte a imagens (o método HTML codifica o texto do link por motivos de segurança). Portanto, eu substituído as chamadas para Html.ActionLink() com chamadas para Url.Action() como este:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

O método Html.ActionLink() renderiza um hiperlink HTML inteiro. O método Url.Action(), por outro lado, processa apenas a URL sem o &lt;um&gt; marca.

Além disso, observe que o novo design inclui guias selecionadas e desmarcadas. Por exemplo, na Figura 8, o **criar novo contato** guia é selecionada e o **Meus contatos** guia não estiver selecionada.


[![A caixa de diálogo Novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Figura 08**: marcados e desmarcados guias ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image16.png))


Para dar suporte à renderização guias selecionadas e desmarcadas, criei um auxiliar HTML personalizado chamado o MenuItemHelper. Esse método auxiliar renderiza a um &lt;li&gt; marca ou uma &lt;classe li = "selecionados"&gt; marca dependendo se o controlador atual e a ação corresponde ao nome do controlador e ação passado para o auxiliar. O código para o MenuItemHelper está contido na listagem 1.

**Listagem 1 - Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

O MenuItemHelper usa a classe TagBuilder internamente para criar o &lt;li&gt; marca HTML. A classe TagBuilder é uma classe de utilitário muito útil que pode ser usado sempre que você precisa para criar uma nova marca HTML. Ele inclui métodos para adicionar atributos, adicionando classes CSS, geração de Ids e modificando a marca s HTML interno.

## <a name="summary"></a>Resumo

Nesta iteração, melhoramos o design visual do nosso aplicativo ASP.NET MVC. Primeiro, você foi apresentado na Galeria de Design do ASP.NET MVC. Você aprendeu a baixar modelos de design livre da Galeria de Design MVC ASP.NET que você pode usar em seus aplicativos ASP.NET MVC.

Em seguida, discutimos como você pode criar um design personalizado, modificando o arquivo de folha de estilos em cascata padrão e o arquivo de paginação do modo de exibição mestre. Para suportar o novo design, precisamos fazer algumas pequenas alterações no nosso aplicativo Contact Manager. Por exemplo, adicionamos um novo auxiliar HTML chamado o MenuItemHelper que exibe as guias selecionadas e.

Na próxima iteração, vamos atacar o assunto muito importante de validação. Vamos adicionar código de validação para o nosso aplicativo para que um usuário não é possível criar um novo contato sem fornecer valores necessários, como uma pessoa s primeiro nome e sobrenome.

> [!div class="step-by-step"]
> [Anterior](iteration-1-create-the-application-cs.md)
> [Próximo](iteration-3-add-form-validation-cs.md)
