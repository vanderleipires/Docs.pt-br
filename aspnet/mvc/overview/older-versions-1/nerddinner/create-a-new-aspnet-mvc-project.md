---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Criar um novo projeto ASP.NET MVC | Microsoft Docs
author: microsoft
description: "Etapa 1 mostra como implementar a estrutura básica do aplicativo NerdDinner."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 4d30a6803b1478014a2afb814ac317df27394446
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="create-a-new-aspnet-mvc-project"></a>Criar um novo projeto ASP.NET MVC
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 1 de livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.
> 
> Etapa 1 mostra como implementar a estrutura básica do aplicativo NerdDinner.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner etapa 1: Arquivo -&gt;novo projeto

Vamos começar nosso aplicativo NerdDinner selecionando o **arquivo -&gt;novo projeto** item de menu no Visual Studio 2008 ou o livre Visual Web Developer 2008 Express.

Isso abrirá a caixa de diálogo "Novo projeto". Para criar um novo aplicativo ASP.NET MVC, vamos selecionar o nó "Web" no lado esquerdo da caixa de diálogo e, em seguida, escolha o modelo de projeto "Aplicativo Web ASP.NET MVC" à direita:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Importante: Verifique se você baixou e instalou o ASP.NET MVC - caso contrário não aparecerão na caixa de diálogo Novo projeto. Você pode usar V2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) se você ainda não instalou-(ASP.NET MVC está disponível na "Web plataforma -&gt;estruturas e tempos de execução" seção).*

Chamaremos o novo projeto, vamos criar "NerdDinner" e, em seguida, clique no botão "okey" para criá-lo.

Quando clicarmos "okey" Visual Studio abrirá uma caixa de diálogo adicional que solicita a opcionalmente, crie um projeto de teste de unidade para o novo aplicativo. Este projeto de teste de unidade nos permite criar testes automatizados que verificam a funcionalidade e o comportamento do nosso aplicativo (algo, abordaremos como tarefas em breve neste tutorial).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

Menu suspenso "Estrutura de teste" na caixa de diálogo acima é populado com todos os disponível ASP.NET MVC unidade teste modelos de projeto instalados no computador. As versões podem ser baixadas para NUnit, MBUnit e XUnit. Também há suporte para a estrutura interna do teste de unidade do Visual Studio.

*Observação: O Visual Studio Unit Test Framework só está disponível com o Visual Studio 2008 Professional e versões posteriores. Se você estiver usando o VS 2008 Standard Edition ou do Visual Web Developer 2008 Express, você precisará baixar e instalar as extensões do NUnit, MBUnit ou XUnit para ASP.NET MVC para essa caixa de diálogo a ser mostrado. A caixa de diálogo não será exibida se não há nenhuma estrutura de teste instalada.*

Vamos usar o nome de "NerdDinner.Tests" padrão para o projeto de teste que criarmos e usar a opção de estrutura "Visual Studio de teste de unidade". Quando clicamos no botão "okey" Visual Studio criará uma solução para nós com dois projetos nele - uma para o nosso aplicativo web e outra para nossos testes de unidade:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Examinando a estrutura de diretório NerdDinner

Quando você cria um novo aplicativo ASP.NET MVC com o Visual Studio, ele adiciona automaticamente um número de arquivos e diretórios para o projeto:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Projetos do ASP.NET MVC por padrão tem seis diretórios de alto nível:

| **Diretório** | **Finalidade** |
| --- | --- |
| **/ Controladores** | Onde você colocou classes do controlador que manipulam solicitações de URL |
| **Ou os modelos** | Onde você colocou classes que representam e manipulam dados |
| **/ Modos de exibição** | Em que você pode colocar arquivos de modelo de interface do usuário que são responsáveis pela saída de renderização |
| **/ Scripts** | Onde colocar os arquivos de biblioteca de JavaScript e scripts (. js) |
| **/ Conteúdo** | Onde você colocou o CSS e arquivos de imagem e outros tipos de conteúdo não dinâmico/não JavaScript |
| **/ Aplicativo\_dados** | Onde armazenar arquivos de dados ser leitura/gravação. |

ASP.NET MVC não requer essa estrutura. Na verdade, desenvolvedores que trabalham em aplicativos grandes normalmente particionará o aplicativo de backup em vários projetos para tornar mais fácil de gerenciar (por exemplo: classes de modelo de dados geralmente vão em um projeto de biblioteca de classe separada do aplicativo web). No entanto, a estrutura de projeto padrão, fornecer uma convenção de diretório padrão adequado que podemos usar para manter nossos preocupações de aplicativo normal.

Quando, expanda a pasta /Controllers descobriremos que o Visual Studio adicionou as duas classes de controlador – HomeController e AccountController – por padrão ao projeto:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Quando, expanda o diretório /Views, descobriremos três subdiretórios – /Home, conta e /Shared –, bem como modelo de vários arquivos dentro deles também foram adicionados ao projeto por padrão:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Quando é expande o /Content e diretórios /Scripts, descobriremos que um arquivo de site que é usado para definir o estilo de todo o HTML no site, bem como bibliotecas JavaScript que podem habilitar o ASP.NET AJAX e jQuery suportem no aplicativo:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Quando, expanda o projeto NerdDinner.Tests descobriremos duas classes que contêm os testes de unidade para nosso classes do controlador:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Esses arquivos padrão adicionados pelo Visual Studio nos fornecem uma estrutura básica para um aplicativo de trabalho - completo com a página inicial, sobre, páginas de logon/logoff/registro de conta e uma página de erro não tratado (todos com fio-up e trabalhar fora da caixa).

### <a name="running-the-nerddinner-application"></a>Executando o aplicativo NerdDinner

Podemos executar o projeto, escolhendo um o **Debug -&gt;iniciar depuração** ou **Debug -&gt;Start Without Debugging** itens de menu:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Isso irá iniciar o servidor Web ASP.NET interno-que vem com o Visual Studio e execute o nosso aplicativo:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Abaixo está a home page do nosso novo projeto (URL: "/") quando ele é executado:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Clique na guia "Sobre" exibe um sobre a página (URL: "/ Home e sobre"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Clicando no link "Logon" na parte superior direita nos leva a uma página de logon (URL: "Conta/LogOn")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Se não temos uma conta de logon, clique no link de registro (URL: "Conta/Register") para criar um:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

O código para implementar o home acima, e logout /Register funcionalidade foi adicionada por padrão, quando criamos nosso novo projeto. Vamos usá-lo como ponto de partida do nosso aplicativo.

### <a name="testing-the-nerddinner-application"></a>Testando o aplicativo NerdDinner

Se estiver usando o Professional Edition ou a versão mais recente do Visual Studio 2008, podemos usar o suporte do IDE do Visual Studio de teste de unidade interna para o projeto de teste:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Escolhendo uma das opções acima, abra o painel de "resultados de teste" dentro do IDE e fornecer com o status de aprovação/reprovação sobre os testes de 27 unidade incluído em nosso novo projeto que abrangem a funcionalidade interna:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Posteriormente neste tutorial vamos falar mais sobre testes automatizados e adicionar testes de unidade adicionais que abrangem a implementamos a funcionalidade do aplicativo.

### <a name="next-step"></a>Próxima etapa

Agora temos uma estrutura de aplicativo básico em vigor. Agora vamos [criar um banco de dados para armazenar os dados de aplicativo](create-a-database.md).

>[!div class="step-by-step"]
[Anterior](introducing-the-nerddinner-tutorial.md)
[Próximo](create-a-database.md)
