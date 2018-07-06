---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Chamar a API da Web de um Windows Phone 8 aplicativo (c#) | Microsoft Docs
author: rmcmurray
description: Crie um cenário de ponta a ponta completo consiste em um aplicativo de API Web ASP.NET que fornece um catálogo de livros de um aplicativo do Windows Phone 8.
ms.author: aspnetcontent
ms.date: 10/09/2013
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 6b7a833818424cbf3a3bf9e1e14e5b2864742c38
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805036"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Chamar API Web em um aplicativo do Windows Phone 8 (c#)
====================
por [Robert McMurray](https://github.com/rmcmurray)

Neste tutorial, você aprenderá como criar um cenário de ponta a ponta completo consiste em um aplicativo de API Web ASP.NET que fornece um catálogo de livros de um aplicativo do Windows Phone 8.

### <a name="overview"></a>Visão geral

Os serviços rESTful, como API Web ASP.NET simplificam a criação de aplicativos baseados em HTTP para os desenvolvedores, abstraindo a arquitetura de aplicativos do lado do servidor e do lado do cliente. Em vez de criar um protocolo proprietário baseado em soquete para comunicação, os desenvolvedores de API da Web simplesmente precisam publicar os métodos HTTP necessários para seu aplicativo (por exemplo: GET, POST, PUT, DELETE), e os desenvolvedores de aplicativos cliente precisam apenas consumir os métodos HTTP que são necessários para seu aplicativo.

Neste tutorial de ponta a ponta, você aprenderá como usar a API da Web para criar os projetos a seguir:

- No [primeira parte deste tutorial](#STEP1), você criará um aplicativo de API Web do ASP.NET que oferece suporte a todas as operações criar, ler, atualizar e excluir (CRUD) para gerenciar um catálogo de livro. Este aplicativo usará o [exemplo de arquivo XML (Books. xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) do MSDN.
- No [segunda parte deste tutorial](#STEP2), você criará um aplicativo interativo do Windows Phone 8 que recupera os dados de seu aplicativo de API da Web.

#### <a name="prerequisites"></a>Pré-requisitos

- Visual Studio 2013 com o Windows Phone 8 SDK instalado
- Windows 8 ou posterior em um sistema de 64 bits com o Hyper-V instalado
- Para obter uma lista de requisitos adicionais, consulte o *requisitos do sistema* seção o [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) página de download.

> [!NOTE]
> Se você pretende testar a conectividade entre a API da Web e projetos do Windows Phone 8 em seu sistema local, você precisará seguir as instruções na *[conectar o emulador do Windows Phone 8 para aplicativos de API da Web em um Local Computador](https://go.microsoft.com/fwlink/?LinkId=324014)* artigo para configurar seu ambiente de teste.


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Etapa 1: Criando o projeto de livraria API da Web

A primeira etapa deste tutorial de ponta a ponta é criar um projeto de API da Web que dá suporte a todas as operações CRUD; Observe que você irá adicionar o projeto de aplicativo do Windows Phone para essa solução em [etapa 2](#STEP2) deste tutorial.

1. Abra **Visual Studio 2013**.
2. Clique em **arquivo**, em seguida, **novos**e então **projeto**.
3. Quando o **novo projeto** caixa de diálogo for exibida, expanda **instalado**, em seguida, **modelos**, em seguida, **Visual c#** e, em seguida, **Web**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Clique na imagem para expandir                                                                |


4. Realçar **aplicativo Web ASP.NET**, insira **livraria** para o nome do projeto e clique **Okey**.
5. Quando o **novo projeto ASP.NET** caixa de diálogo for exibida, selecione o **API da Web** modelo e clique **Okey**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Clique na imagem para expandir                                                                |


6. Quando o projeto de API da Web é aberto, remover o controlador de exemplo do projeto:

    1. Expanda o **controladores** pasta no Gerenciador de soluções.
    2. Clique com botão direito do **ValuesController.cs** do arquivo e, em seguida, clique em **excluir**.
    3. Clique em **Okey** quando for solicitado a confirmar a exclusão.
7. Adicionar um arquivo de dados XML ao projeto de API da Web; Esse arquivo contém o conteúdo do catálogo livraria:

   1. Com o botão direito do **App\_dados** pasta no Gerenciador de soluções, em seguida, clique em **Add**e, em seguida, clique em **Novo Item**.
   2. Quando o **Adicionar Novo Item** caixa de diálogo é exibida, realce o **arquivo XML** modelo.
   3. Nomeie o arquivo **Books**e, em seguida, clique em **Add**.
   4. Quando o **Books** arquivo é aberto, substitua o código no arquivo com o XML de exemplo **Books** arquivo no MSDN: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Salve e feche o arquivo XML.

8. Adicionar o modelo de livraria ao projeto de API da Web; Este modelo contém a lógica de criar, ler, atualizar e excluir (CRUD) para o aplicativo livraria:

   1. Clique com botão direito do **modelos** pasta no Gerenciador de soluções, em seguida, clique em **Add**e, em seguida, clique em **classe**.
   2. Quando o **Adicionar Novo Item** caixa de diálogo for exibida, nomeie o arquivo de classe **BookDetails.cs**e, em seguida, clique em **adicionar**.
   3. Quando o **BookDetails.cs** arquivo é aberto, substitua o código no arquivo pelo seguinte: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Salve e feche o **BookDetails.cs** arquivo.

9. Adicione o controlador de livraria ao projeto de API da Web:

   1. Com o botão direito do **controladores** pasta no Gerenciador de soluções, em seguida, clique em **Add**e, em seguida, clique em **controlador**.
   2. Quando o **adicionar Scaffold** caixa de diálogo for exibida, realçar **controlador Web API 2 - vazio**e, em seguida, clique em **adicionar**.
   3. Quando o **Adicionar controlador** caixa de diálogo for exibida, nomeie o controlador **BooksController**e, em seguida, clique em **adicionar**.
   4. Quando o **BooksController.cs** arquivo é aberto, substitua o código no arquivo pelo seguinte: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Salve e feche o **BooksController.cs** arquivo.

10. Compile o aplicativo de API da Web para verificar se há erros.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Etapa 2: Adicionar o projeto de catálogo do Windows Phone 8 livraria

É a próxima etapa deste cenário de ponta a ponta criar o aplicativo de catálogo para o Windows Phone 8. Este aplicativo usará o *Windows Phone Databound App* modelo para a interface do usuário padrão e ele usará o aplicativo de API da Web que você criou na [etapa 1](#STEP1) deste tutorial, como a fonte de dados.

1. Clique com botão direito a **livraria** solução no no Gerenciador de soluções, clique em **Add**e, em seguida, **novo projeto**.
2. Quando o **novo projeto** caixa de diálogo for exibida, expanda **instalado**, em seguida, **Visual c#** e, em seguida, **Windows Phone**.
3. Realçar **Windows Phone Databound App**, insira **BookCatalog** para o nome e, em seguida, clique **Okey**.
4. Adicione o pacote do NuGet de Json.NET para o **BookCatalog** projeto:

    1. Clique com botão direito **referências** para o **BookCatalog** do projeto no Gerenciador de soluções e, em seguida, clique em **Manage NuGet Packages**.
    2. Quando o **gerenciar pacotes NuGet** caixa de diálogo for exibida, expanda o **Online** seção e realçar **nuget.org**.
    3. Insira **Json.NET** na pesquisa de campo e clique no ícone de pesquisa.
    4. Realçar **Json.NET** nos resultados da pesquisa e clique **instalar**.
    5. Quando a instalação for concluída, clique em **fechar**.
5. Adicionar o **BookDetails** modelo para o **BookCatalog** do projeto; ele contém um modelo genérico da classe livraria:

   1. Clique com botão direito do **BookCatalog** do projeto no Gerenciador de soluções, clique em **Add**e, em seguida, clique em **nova pasta**.
   2. Nomeie a nova pasta **modelos**.
   3. Clique com botão direito do **modelos** pasta no Gerenciador de soluções, em seguida, clique em **Add**e, em seguida, clique em **classe**.
   4. Quando o **Adicionar Novo Item** caixa de diálogo for exibida, nomeie o arquivo de classe **BookDetails.cs**e, em seguida, clique em **adicionar**.
   5. Quando o **BookDetails.cs** arquivo é aberto, substitua o código no arquivo pelo seguinte: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Salve e feche o **BookDetails.cs** arquivo.

6. Atualizar o **MainViewModel.cs** classe para incluir a funcionalidade para se comunicar com o aplicativo de API da Web de livraria:

   1. Expanda o **ViewModels** pasta no Gerenciador de soluções e, em seguida, clique duas vezes o **MainViewModel.cs** arquivo.
   2. Quando o **MainViewModel.cs** arquivo é aberto, substitua o código no arquivo pelo seguinte; Observe que você precisará atualizar o valor da `apiUrl` constante com a URL real de sua API da Web: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Salve e feche o **MainViewModel.cs** arquivo.

7. Atualizar o **MainPage. XAML** arquivo para personalizar o nome do aplicativo:

   1. Clique duas vezes o **MainPage. XAML** arquivo no Gerenciador de soluções.
   2. Quando o **MainPage. XAML** arquivo é aberto, localize as seguintes linhas de código: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Substitua as linhas com o seguinte: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Salve e feche o **MainPage. XAML** arquivo.

8. Atualizar o **DetailsPage.xaml** arquivo para personalizar os itens exibidos:

   1. Clique duas vezes o **DetailsPage.xaml** arquivo no Gerenciador de soluções.
   2. Quando o **DetailsPage.xaml** arquivo é aberto, localize as seguintes linhas de código: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Substitua as linhas com o seguinte: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Salve e feche o **DetailsPage.xaml** arquivo.

9. Compile o aplicativo do Windows Phone para verificar se há erros.

### <a name="step-3-testing-the-end-to-end-solution"></a>Etapa 3: Testar a solução de ponta a ponta

Conforme mencionado na *pré-requisitos* projetos de seção deste tutorial, quando você estiver testando a conectividade entre a API da Web e Windows Phone 8 em seu sistema local, você precisará seguir as instruções no *[ Conectar o emulador do Windows Phone 8 para aplicativos de API da Web em um computador Local](https://go.microsoft.com/fwlink/?LinkId=324014)* artigo para configurar seu ambiente de teste.

Quando você tiver configurado o ambiente de teste, você precisará definir o aplicativo do Windows Phone como projeto de inicialização. Para fazer isso, realce o **BookCatalog** aplicativo no Gerenciador de soluções e clique **definir como projeto de inicialização**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Clique na imagem para expandir |

Quando você pressiona F5, o Visual Studio iniciará os dois o Windows Phone emulador, que exibirá uma &quot;Aguarde&quot; da mensagem enquanto os dados de aplicativo são recuperados de sua API da Web:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Clique na imagem para expandir |

Se tudo for bem-sucedido, você deverá ver o catálogo exibido:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Clique na imagem para expandir |

Se você tocar em qualquer título de livro, o aplicativo exibirá a descrição de catálogo:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Clique na imagem para expandir |

Se o aplicativo não pode se comunicar com sua API Web, será exibida uma mensagem de erro:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Clique na imagem para expandir |

Se você tocar na mensagem de erro, todos os detalhes adicionais sobre o erro serão exibidos:


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Clique na imagem para expandir                                                                 |

