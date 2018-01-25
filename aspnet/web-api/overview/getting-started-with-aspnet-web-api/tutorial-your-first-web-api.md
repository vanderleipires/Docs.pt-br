---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: "Introdução ao ASP.NET Web API 2 (c#)"
author: MikeWasson
description: "HTTP não é apenas para serviços de páginas da web. Também é uma plataforma poderosa para a criação de APIs que expõem os dados e serviços. O HTTP é simples, flexível e ubiq..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 6ff9fd279a03197f761454bba3f180d7428b1b1f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-web-api-2-c"></a>Introdução ao ASP.NET Web API 2 (c#)
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP não é apenas para serviços de páginas da web. HTTP também é uma plataforma poderosa para a criação de APIs que expõem os dados e serviços. O HTTP é simples, flexível e de todos os lugares. Praticamente qualquer plataforma que você pode pensar tem uma biblioteca HTTP para que serviços HTTP podem atingir uma ampla gama de clientes, incluindo navegadores, dispositivos móveis e aplicativos de área de trabalho tradicionais.
 
API da Web do ASP.NET é uma estrutura para a criação de APIs na parte superior do .NET Framework da web. Neste tutorial, você usará a API da Web do ASP.NET para criar uma web API que retorna uma lista de produtos.

 
 ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
  
 - [Visual Studio 2017](https://www.visualstudio.com/downloads/)
 - Web API 2

Consulte [criar uma API da web com o Visual Studio para Windows e o ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) para uma versão mais recente deste tutorial.

## <a name="create-a-web-api-project"></a>Criar um projeto de API da Web

Neste tutorial, você usará a API da Web do ASP.NET para criar uma web API que retorna uma lista de produtos. A página da web front-end usa jQuery para exibir os resultados.

![](tutorial-your-first-web-api/_static/image1.png)

Inicie o Visual Studio e selecione **novo projeto** do **iniciar** página. Ou, do **arquivo** menu, selecione **novo** e **projeto**.

No **modelos** painel, selecione **modelos instalados** e expanda o **Visual C#** nó. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web ASP.NET**. Nomeie o projeto "ProductsApp" e clique em **Okey**.

![](tutorial-your-first-web-api/_static/image2.png)

No **novo projeto ASP.NET** caixa de diálogo, selecione o **vazio** modelo. Em &quot;adicionar pastas e referências de núcleo&quot;, verifique **API da Web**. Clique em **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Você também pode criar um projeto de API da Web usando o &quot;API da Web&quot; modelo. O modelo de API da Web usa o ASP.NET MVC para fornecer páginas de Ajuda da API. Estou usando o modelo vazio para este tutorial porque Mostrar API da Web sem MVC. Em geral, você não precisa saber o ASP.NET MVC para usar a API da Web.


## <a name="adding-a-model"></a>Adicionando um modelo

Um *modelo* é um objeto que representa os dados em seu aplicativo. ASP.NET Web API pode serializar automaticamente seu modelo para outro formato, XML ou JSON e, em seguida, gravar os dados serializados no corpo da mensagem de resposta HTTP. Como um cliente pode ler o formato de serialização, ele pode desserializar o objeto. A maioria dos clientes pode analisar XML ou JSON. Além disso, o cliente pode indicar que ele deseja definindo o cabeçalho Accept na mensagem de solicitação HTTP de formato.

Vamos começar criando um modelo simples que representa um produto.

Se o Gerenciador de soluções não estiver visível, clique no **exibição** menu e selecione **Gerenciador de soluções**. No Gerenciador de soluções, clique na pasta de modelos. No menu de contexto, selecione **adicionar** , em seguida, selecione **classe**.

![](tutorial-your-first-web-api/_static/image4.png)

Nomeie a classe &quot;produto&quot;. Adicione as seguintes propriedades para o `Product` classe.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Adicionando um controlador

Na API da Web, um *controlador* é um objeto que manipula as solicitações HTTP. Vamos adicionar um controlador que pode retornar uma lista de produtos ou um único produto especificado por ID.

> [!NOTE]
> Se você tiver usado o ASP.NET MVC, você já está familiarizados com os controladores. Controladores de API da Web são semelhantes aos controladores MVC, mas herdam o **ApiController** classe o **controlador** classe.

Em **Solution Explorer**, clique na pasta controladores. Selecione **adicionar** e, em seguida, selecione **controlador**.

![](tutorial-your-first-web-api/_static/image5.png)

No **adicionar Scaffold** caixa de diálogo, selecione **controlador de API da Web - vazio**. Clique em **Adicionar**.

![](tutorial-your-first-web-api/_static/image6.png)

No **Adicionar controlador** caixa de diálogo, o nome do controlador &quot;ProductsController&quot;. Clique em **Adicionar**.

![](tutorial-your-first-web-api/_static/image7.png)

O scaffolding cria um arquivo chamado ProductsController.cs na pasta controladores.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Não é necessário colocar os controladores em uma pasta chamada controladores. O nome da pasta é apenas uma maneira conveniente de organizar seus arquivos de origem.


Se esse arquivo não estiver aberto, clique duas vezes no arquivo para abri-lo. Substitua o código neste arquivo com o seguinte:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Para manter o exemplo simples, os produtos são armazenados em uma matriz fixa dentro da classe do controlador. É claro que, em um aplicativo real, você consulta um banco de dados ou usar outra fonte de dados externa.

O controlador define dois métodos que retornam produtos:

- O `GetAllProducts` método retorna a lista completa de produtos como um **IEnumerable&lt;produto&gt;**  tipo.
- O `GetProduct` método procura um único produto por sua ID.

É só isso! Você tem uma API da web do trabalho. Cada método do controlador corresponde a um ou mais URIs:

| Método do controlador | URI |
| --- | --- |
| GetAllProducts | produtos/api / |
| GetProduct | /api/products/*id* |

Para o `GetProduct` método, o *id* no URI é um espaço reservado. Por exemplo, para obter o produto com ID 5, o URI é `api/products/5`.

Para obter mais informações sobre como a API da Web encaminha solicitações HTTP para os métodos do controlador, consulte [roteamento na API da Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Chamar a API da Web com Javascript e jQuery

Nesta seção, vamos adicionar uma página HTML que usa AJAX para chamar a API da web. Vamos usar jQuery para fazer chamadas AJAX e também para atualizar a página com os resultados.

No Gerenciador de soluções, clique com o botão direito e selecione **adicionar**, em seguida, selecione **Novo Item**.

![](tutorial-your-first-web-api/_static/image9.png)

No **Adicionar Novo Item** caixa de diálogo, selecione o **Web** nó **Visual C#**e, em seguida, selecione o **página HTML** item. Nomeie a página &quot;index.html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Substitua tudo neste arquivo com o seguinte:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Há várias maneiras de obter jQuery. Neste exemplo, usei a [Microsoft Ajax CDN](../../../ajax/cdn/overview.md). Você também pode baixá-lo do [http://jquery.com/](http://jquery.com/)e o ASP.NET "API Web" jQuery também inclui o modelo de projeto.

### <a name="getting-a-list-of-products"></a>Obtendo uma lista de produtos

Para obter uma lista de produtos, envie uma solicitação HTTP GET para &quot;/api/produtos&quot;.

O jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) função envia uma solicitação AJAX. Para a resposta contém uma matriz de objetos JSON. O `done` função especifica um retorno de chamada que é chamado quando a solicitação for bem-sucedida. O retorno de chamada, atualizamos o DOM com as informações de produto.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Um produto por ID

Para obter um produto por ID, envie uma solicitação HTTP GET para &quot;/api/produtos/*id*&quot;, onde *id* é a ID do produto.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Chamamos ainda `getJSON` para enviar a solicitação AJAX, mas desta vez colocamos a ID no URI de solicitação. A resposta dessa solicitação é uma representação JSON de um único produto.

## <a name="running-the-application"></a>Executando o aplicativo

Pressione F5 para iniciar a depuração do aplicativo. A página da web deve parecer com o seguinte:

![](tutorial-your-first-web-api/_static/image11.png)

Para obter um produto por ID, insira a ID e clique em Pesquisar:

![](tutorial-your-first-web-api/_static/image12.png)

Se você inserir uma ID inválida, o servidor retornará um erro HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Usando a tecla F12 para exibir a solicitação e resposta HTTP

Quando você estiver trabalhando com um serviço HTTP, ele pode ser muito útil ver a solicitação HTTP e mensagens de solicitação. Você pode fazer isso usando as ferramentas de desenvolvedor F12 no Internet Explorer 9. No Internet Explorer 9, pressione **F12** para abrir as ferramentas. Clique o **rede** guia e pressione **iniciar captura**. Agora vá para a página da web e pressione **F5** para recarregar a página da web. Internet Explorer capturará o tráfego HTTP entre o navegador e o servidor web. O modo de exibição de resumo mostra todo o tráfego de rede para uma página:

![](tutorial-your-first-web-api/_static/image14.png)

Localize a entrada para o URI relativo "api/produtos /". Selecione esta opção e clique em **acesse a exibição detalhada**. Na exibição detalhes, há guias para exibir os corpos e cabeçalhos de solicitação e resposta. Por exemplo, se você clicar no **cabeçalhos de solicitação** guia, você pode ver que o cliente solicitou &quot;aplicativo/json&quot; no cabeçalho Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Se você clicar na guia de corpo de resposta, você pode ver como a lista de produtos foi serializada como JSON. Outros navegadores têm funcionalidade semelhante. Outra ferramenta útil é [Fiddler](http://www.fiddler2.com/fiddler2/), um web proxy de depuração. Você pode usar o Fiddler para exibir o tráfego HTTP e também para compor as solicitações HTTP, que lhe dá controle total sobre os cabeçalhos HTTP na solicitação.

## <a name="see-this-app-running-on-azure"></a>Consulte este aplicativo em execução no Azure

Você gostaria de ver o site concluído em execução como um aplicativo web em tempo real? Você pode implantar uma versão completa do aplicativo para sua conta do Azure, clicando no botão a seguir.

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Você precisa de uma conta do Azure para implantar essa solução no Azure. Se você não tiver uma conta, você tem as seguintes opções:

- [Abra uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos você pode usar para experimentar os serviços do Azure pagos e mesmo depois que eles são usados até que você pode manter a conta e livre de usar os serviços do Azure.
- [Ativar os benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -assinatura do MSDN fornece créditos de cada mês em que você pode usar para os serviços do Azure pagos.

## <a name="next-steps"></a>Próximas etapas

- Para obter um exemplo mais completo de um serviço HTTP que dá suporte a ações de POST, PUT e DELETE e grava em um banco de dados, consulte [usando API Web 2 com o Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Para obter mais informações sobre como criar aplicativos web flexível e responsivo sobre um serviço HTTP, consulte [único aplicativo de página ASP.NET](../../../single-page-application/index.md).
- Para obter informações sobre como implantar um projeto da web do Visual Studio para o serviço de aplicativo do Azure, consulte [criar um aplicativo web ASP.NET no serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
