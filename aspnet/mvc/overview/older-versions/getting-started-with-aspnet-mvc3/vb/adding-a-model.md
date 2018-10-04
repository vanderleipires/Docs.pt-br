---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Adicionando um modelo (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b1370148018faa8c6c884251bfa86761f45d7e49
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576996"
---
<a name="adding-a-model-vb"></a>Adicionando um modelo (VB)
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução de ferramentas de suporte +)
> 
> Se você estiver usando o Visual Studio 2010, em vez do Visual Web Developer 2010, instale os pré-requisitos, clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com código-fonte VB.NET está disponível para acompanhar este tópico. [Baixe a versão do VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir o c#, alterne para o [c# versão](../cs/adding-a-model.md) deste tutorial.


## <a name="adding-a-model"></a>Adicionando um modelo

Nesta seção, você adicionará algumas classes para o gerenciamento de filmes em um banco de dados. Essas classes será a parte de "modelo" do aplicativo ASP.NET MVC.

Você usará uma tecnologia de acesso a dados do .NET Framework conhecida como o Entity Framework para definir e trabalhar com essas classes de modelo. A Entity Framework (também conhecido como EF) oferece suporte a um paradigma de desenvolvimento chamado *Code First*. Código primeiro permite que você crie objetos de modelo ao escrever classes simples. (Esses são também conhecidas como classes POCO, de "objetos CLR do BOM e velho".) Em seguida, você pode ter o banco de dados criado em tempo real de suas classes, que permite que um fluxo de trabalho de desenvolvimento muito claro e rápido.

## <a name="adding-model-classes"></a>Adicionando Classes de modelo

Na **Gerenciador de soluções**, clique com botão direito do *modelos* pasta, selecione **Add**e, em seguida, selecione **classe**.

![](adding-a-model/_static/image1.png)

Nomeie a classe "Filme".

Adicione as seguintes propriedades de cinco para o `Movie` classe:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Vamos usar o `Movie` classe para representar os filmes em um banco de dados. Cada instância de um `Movie` objeto corresponderá a uma linha em uma tabela de banco de dados e cada propriedade do `Movie` classe será mapeado para uma coluna na tabela.

No mesmo arquivo, adicione o seguinte `MovieDBContext` classe:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

O `MovieDBContext` classe representa o contexto de banco de dados de filme do Entity Framework, que lida com a busca, armazenar e atualizar `Movie` instâncias em um banco de dados de classe. O `MovieDBContext` deriva o `DbContext` fornecido pela estrutura de entidades de classe base. Para obter mais informações sobre `DbContext` e `DbSet`, consulte [melhorias de produtividade para o Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Para poder referenciar `DbContext` e `DbSet`, você precisará adicionar o seguinte `imports` instrução na parte superior do arquivo:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

A conclusão *Movie.vb* arquivo é mostrado abaixo.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Criando uma cadeia de Conexão e trabalhar com o SQL Server Compact

O `MovieDBContext` classe criada por você lida com a tarefa de conectar-se ao banco de dados e mapeamento `Movie` objetos para os registros de banco de dados. Uma pergunta que você pode fazer, no entanto, é como especificar qual banco de dados que ele se conectará. Você terá de fazer isso adicionando as informações de conexão na *Web. config* arquivo do aplicativo.

Abra a raiz do aplicativo *Web. config* arquivo. (Não o *Web. config* arquivo na *modos de exibição* pasta.) A imagem a seguir mostrar ambos *Web. config* arquivos; abrir os *Web. config* arquivo circulados em vermelho.

![](adding-a-model/_static/image2.png)

## 

Adicione a seguinte cadeia de conexão para o `<connectionStrings>` elemento na *Web. config* arquivo.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

O exemplo a seguir mostra uma parte do *Web. config* arquivo com a cadeia de caracteres de conexão nova adicionada:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Essa pequena quantidade de código e o XML é tudo o que precisa ser escrita para representar e armazenar os dados de filmes em um banco de dados.

Em seguida, você criará um novo `MoviesController` classe que você pode usar para exibir os dados de filme e permitir que usuários criem novas listagens de filme.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)
