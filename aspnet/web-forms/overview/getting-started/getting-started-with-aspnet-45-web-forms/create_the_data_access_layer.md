---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Criar camada de acesso a dados | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para nós...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 5b4bb5bc89938836bc37e8ebd385fa966bc6f511
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390014"
---
<a name="create-the-data-access-layer"></a>Criar camada de acesso a dados
====================
por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Baixe o livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Este tutorial descreve como criar, acessar e analisar dados de um banco de dados usando o Web Forms do ASP.NET e o Entity Framework Code First. Este tutorial se baseia no tutorial anterior, "Criar o projeto" e faz parte da série de tutoriais de Wingtip Toys Store. Depois de concluir este tutorial, você terá criado um grupo de classes de acesso a dados que estão na *modelos* pasta do projeto.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como criar os modelos de dados.
- Como inicializar e propagar o banco de dados.
- Como atualizar e configurar o aplicativo para dar suporte ao banco de dados.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Estes são os recursos introduzidos no tutorial:

- Entity Framework Code First
- LocalDB
- Anotações de dados

## <a name="creating-the-data-models"></a>Criando modelos de dados

[Entity Framework](https://msdn.microsoft.com/data/aa937723) é uma estrutura de mapeamento relacional de objeto (ORM). Ele permite trabalhar com dados relacionais como objetos, eliminando a maioria do código de acesso a dados que você normalmente precisa escrever. Usando o Entity Framework, você pode emitir consultas usando [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), em seguida, recuperar e manipular dados como objetos fortemente tipados. O LINQ fornece padrões para consultar e atualizar dados. Usando o Entity Framework permite que você se concentre na criação do restante do seu aplicativo, em vez de enfocar conceitos básicos de acesso os dados. Mais tarde nessa série de tutoriais, mostraremos como usar os dados para preencher as consultas de produto e de navegação.

Entity Framework oferece suporte a um paradigma de desenvolvimento chamado *Code First*. O Code First permite definir modelos de dados usando classes. Uma classe é um constructo que permite que você crie seus próprios tipos personalizados através do agrupamento de variáveis de outros tipos, métodos e eventos. Você pode mapear as classes para um banco de dados existente ou usá-los para gerar um banco de dados. Neste tutorial, você criará os modelos de dados, escrevendo classes de modelo de dados. Em seguida, você vai permitir que Entity Framework criar o banco de dados em tempo real dessas novas classes.

Você começará criando as classes de entidade que definem os modelos de dados para o aplicativo de formulários da Web. Em seguida, você criará uma classe de contexto que gerencia as classes de entidade e fornece acesso a dados no banco de dados. Você também criará uma classe de inicializador que você usará para popular o banco de dados.

### <a name="entity-framework-and-references"></a>Referências e o entity Framework

Por padrão, Entity Framework está incluído quando você cria um novo **aplicativo Web ASP.NET** usando o **Web Forms** modelo. Entity Framework pode ser instalado, desinstalado e atualizado como um pacote do NuGet.

Este pacote NuGet inclui os seguintes **tempo de execução** assemblies dentro de seu projeto:

- EntityFramework – todo o código de tempo de execução comum usado pelo Entity Framework
- EntityFramework – o provedor do Microsoft SQL Server para o Entity Framework

### <a name="entity-classes"></a>Classes de entidade

Classes que você criou para definir o esquema dos dados são chamadas de classes de entidade. Se você for novo no design de banco de dados, considere as classes de entidade como definições de tabela de banco de dados. Cada propriedade na classe especifica uma coluna na tabela do banco de dados. Essas classes fornecem uma interface leve entre código orientada a objeto e a estrutura de tabela relacional do banco de dados relacional de objeto.

Neste tutorial, você começará adicionando classes de entidade simples que representam os esquemas para produtos e as categorias. A classe de produtos conterá definições para cada produto. O nome de cada um dos membros da classe product será `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, e `Category`. A classe da categoria conterá definições para cada categoria de um produto pode pertencer, como o carro, barco ou plano. O nome de cada um dos membros da classe categoria será `CategoryID`, `CategoryName`, `Description`, e `Products`. Cada produto pertence a uma das categorias. Essas classes de entidade serão adicionadas para o projeto existente *modelos* pasta.

1. No **Gerenciador de soluções**, com o botão direito do *modelos* pasta e, em seguida, selecione **Add**  - &gt; **Novo Item**. 

    ![Criar camada de acesso a dados - novo Menu de Item](create_the_data_access_layer/_static/image1.png)

   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Sob **Visual c#** da **instalado** painel à esquerda, selecione **código**. 

    ![Criar camada de acesso a dados - novo Menu de Item](create_the_data_access_layer/_static/image2.png)
3. Selecione **classe** no painel central e nomeie essa nova classe *Product.cs*.
4. Clique em **Adicionar**.  
   O novo arquivo de classe é exibido no editor.
5. Substitua o código padrão pelo código a seguir:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Criar outra classe, repetindo as etapas 1 a 4, no entanto, nomeie a nova classe *Category.cs* e substitua o código padrão pelo código a seguir:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Como mencionado anteriormente, o `Category` classe representa o tipo de produto que o aplicativo é projetado para vender (como <a id="a"> </a> &quot;carros&quot;, &quot;barcos&quot;, &quot;Rockets&quot;e assim por diante) e o `Product` classe representa os produtos individuais (toys) no banco de dados. Cada instância de um `Product` objeto corresponderá a uma linha dentro de uma tabela de banco de dados relacional, e cada propriedade da classe Product será mapeado para uma coluna na tabela de banco de dados relacional. Mais tarde neste tutorial, você revisará os dados contidos no banco de dados do produto.

### <a name="data-annotations"></a>Anotações de dados

Você deve ter notado que determinados membros das classes têm atributos especificando detalhes sobre o membro, como `[ScaffoldColumn(false)]`. Esses são *anotações de dados*. Os atributos de anotação de dados podem descrever como validar a entrada do usuário para esse membro, para especificar a formatação para ele e especificar como ela é modelada quando o banco de dados é criado.

### <a name="context-class"></a>Classe Context

Para começar a usar as classes para acesso a dados, você deve definir uma classe de contexto. Conforme mencionado anteriormente, a classe de contexto gerencia as classes de entidade (como o `Product` classe e o `Category` classe) e fornece acesso a dados no banco de dados.

Este procedimento adiciona uma novo contexto classe c# para o *modelos* pasta.

1. Clique com botão direito do *modelos* pasta e, em seguida, selecione **Add**  - &gt; **Novo Item**.   
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione **classe** do painel do meio, nomeie- *ProductContext.cs* e clique em **adicionar**.
3. Substitua o código padrão contido na classe pelo código a seguir:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Esse código adiciona o `System.Data.Entity` namespace para que você tenha acesso a toda a funcionalidade de núcleo do Entity Framework, que inclui a capacidade de consulta, inserir, atualizar e excluir dados trabalhando com objetos com rigidez de tipos.

O `ProductContext` classe representa o contexto de banco de dados de produto do Entity Framework, que lida com a busca, armazenar e atualizar `Product` instâncias no banco de dados de classe. O `ProductContext` classe deriva de `DbContext` fornecido pelo Entity Framework de classe base.

### <a name="initializer-class"></a>Classe de inicializador

Você precisará executar alguma lógica personalizada para inicializar o tempo de banco de dados primeiro que o contexto é usado. Isso permitirá que os dados de propagação a ser adicionado ao banco de dados para que você possa exibir imediatamente categorias e produtos.

Este procedimento adiciona uma novo inicializador classe c# para o *modelos* pasta.

1. Criar outra `Class` no *modelos* pasta e nomeie-o *ProductDatabaseInitializer.cs*.
2. Substitua o código padrão contido na classe pelo código a seguir:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Como você pode ver no código acima, quando o banco de dados é criado e inicializado, o `Seed` propriedade é substituída e definida. Quando o `Seed` estiver definida, os valores das categorias e produtos são usados para preencher o banco de dados. Se você tentar atualizar os dados de semente, modificando o código acima, depois que o banco de dados for criado, você não verá todas as atualizações quando você executar o aplicativo Web. O motivo é que o código acima usa uma implementação de `DropCreateDatabaseIfModelChanges` classe reconhecer se o modelo (esquema) foi alterado antes de redefinir os dados de semente. Se nenhuma alteração é feita para o `Category` e `Product` classes de entidade, o banco de dados não serão reinicializadas com os dados de semente.

> [!NOTE] 
> 
> Se você quisesse que o banco de dados a ser recriado sempre que você executou o aplicativo, você pode usar o `DropCreateDatabaseAlways` classe, em vez do `DropCreateDatabaseIfModelChanges` classe. No entanto para esta série de tutoriais, use o `DropCreateDatabaseIfModelChanges` classe.


Neste momento neste tutorial, você terá um *modelos* pasta com quatro novas classes e a classe de um padrão:

![Criar pasta de modelos de camada de acesso a dados-](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Configurando o aplicativo para usar o modelo de dados

Agora que você criou as classes que representam os dados, você deve configurar o aplicativo para usar as classes. No *global. asax* arquivo, que você adicione o código que inicializa o modelo. No *Web. config* arquivo que você adicione informações que informa ao aplicativo de qual banco de dados você usará para armazenar os dados que são representados por novas classes de dados. O *global. asax* arquivo pode ser usado para manipular eventos de aplicativo ou métodos. O *Web. config* arquivo permite que você controle a configuração do seu aplicativo web ASP.NET.

#### <a name="updating-the-globalasax-file"></a>Atualizando o arquivo global. asax

Para inicializar os modelos de dados quando o aplicativo é iniciado, você atualizará o `Application_Start` manipulador na *Global.asax.cs* arquivo.

> [!NOTE] 
> 
> No Solution Explorer, você pode selecionar o *global. asax* arquivo ou o *Global.asax.cs* arquivo para editar o *Global.asax.cs* arquivo.


1. Adicione o seguinte código realçado em amarelo para o `Application_Start` método na *Global.asax.cs* arquivo.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Seu navegador deve dar suporte a HTML5 para exibir o código realçado em amarelo, ao exibir esta série de tutoriais em um navegador.


Conforme mostrado no código acima, quando o aplicativo é iniciado, o aplicativo especifica o inicializador que será executado durante a primeira vez que os dados é acessado. Os dois namespaces adicionais são necessários para acessar o `Database` objeto e o `ProductDatabaseInitializer` objeto.

 Modificando o arquivo Web. config 

Embora o Entity Framework Code First irá gerar um banco de dados para você em um local padrão quando o banco de dados é preenchido com dados de semente, adicionando suas próprias informações de conexão ao seu aplicativo lhe dá controle sobre o local do banco de dados. Você especifica essa conexão de banco de dados usando uma cadeia de conexão na caixa de diálogo *Web. config* arquivo na raiz do projeto. Adicionando uma nova cadeia de caracteres de conexão, você pode direcionar o local do banco de dados (*wingtiptoys.mdf*) a ser criado no diretório de dados do aplicativo (*App\_dados*), em vez de seu padrão local. Essa alteração permitirá que você localize e inspecione o arquivo de banco de dados posteriormente no tutorial.

1. Na **Gerenciador de soluções**, encontre e abra o *Web. config* arquivo.
2. Adicione a cadeia de conexão realçada em amarelo para o `<connectionStrings>` seção o *Web. config* arquivos da seguinte maneira:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Quando o aplicativo é executado pela primeira vez, ele criará o banco de dados no local especificado pela cadeia de caracteres de conexão. Mas, antes de executar o aplicativo, vamos compilá-lo pela primeira vez.

## <a name="building-the-application"></a>Compilando o aplicativo

Para certificar-se de que todas as classes e as alterações ao seu aplicativo Web funcionem, você deve compilar o aplicativo.

1. Dos **Debug** menu, selecione **WingtipToys compilar**.  
 O **saída** janela for exibida, e se tudo correr bem, você verá um *bem-sucedida* mensagem.  

    ![Criar camada de acesso a dados - Windows de saída](create_the_data_access_layer/_static/image4.png)

Se você tiver um erro, verifique novamente as etapas acima. As informações de **saída** janela indicará qual arquivo tem um problema e onde uma alteração é necessária no arquivo. Essas informações permitem determinar qual parte das etapas acima precisam ser revisados e corrigidos no seu projeto.

## <a name="summary"></a>Resumo

Neste tutorial da série de você ter criado o modelo de dados, bem como, adicionado o código que será usado para inicializar e propagar o banco de dados. Você também configurou o aplicativo para usar os modelos de dados quando o aplicativo é executado.

No próximo tutorial, você adicionará atualizar a interface do usuário, navegação e recuperar dados do banco de dados. Isso resultará no banco de dados que estão sendo criado automaticamente com base nas classes de entidade que você criou neste tutorial.

## <a name="additional-resources"></a>Recursos adicionais

[Visão geral do Entity Framework](https://msdn.microsoft.com/library/bb399567.aspx)   
[Guia do Iniciante para o ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[Código primeiro desenvolvimento com o Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (vídeo)   
[API Fluent do Code First relações](https://msdn.microsoft.com/data/hh134698)   
[Anotações de dados Code First](https://msdn.microsoft.com/data/gg193958)  
[Aprimoramentos de produtividade para o Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Anterior](create-the-project.md)
> [Próximo](ui_and_navigation.md)
