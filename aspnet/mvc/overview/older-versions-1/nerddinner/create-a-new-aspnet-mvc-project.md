---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Criar um novo projeto ASP.NET MVC | Microsoft Docs
author: microsoft
description: Etapa 1 mostra como colocar a estrutura básica do aplicativo NerdDinner em vigor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 9f5e0b3f82d113fc72ab4002ec8d06ad8444dceb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374270"
---
<a name="create-a-new-aspnet-mvc-project"></a>Criar um novo projeto ASP.NET MVC
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 1 de um livre [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.
> 
> Etapa 1 mostra como colocar a estrutura básica do aplicativo NerdDinner em vigor.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-1-file-gtnew-project"></a>Etapa 1 do NerdDinner: Arquivo -&gt;novo projeto

Vamos começar nosso aplicativo NerdDinner, selecionando o **arquivo -&gt;novo projeto** item de menu dentro do Visual Studio 2008 ou o gratuito Visual Web Developer 2008 Express.

Isso abrirá a caixa de diálogo "Novo projeto". Para criar um novo aplicativo ASP.NET MVC, vamos vai selecionar o nó "Web" no lado esquerdo da caixa de diálogo e, em seguida, escolha o modelo de projeto "Aplicativo Web ASP.NET MVC" à direita:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Importante: Certifique-se de que você baixou e instalou o ASP.NET MVC - caso contrário, ele não aparecerá na caixa de diálogo Novo projeto. Você pode usar V2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) se você ainda não instalou-lo (ASP.NET MVC está disponível dentro a "plataforma Web -&gt;estruturas e tempos de execução" seção).*

Chamaremos o novo projeto, vamos criar "NerdDinner" e, em seguida, clique no botão "okey" para criá-lo.

Quando clicamos em "okey" Visual Studio abrirá uma caixa de diálogo adicional que nos solicita, opcionalmente, crie um projeto de teste de unidade para o novo aplicativo. Este projeto de teste de unidade nos permite criar testes automatizados que verifiquem a funcionalidade e o comportamento do nosso aplicativo (algo que abordaremos como tarefas pendentes posteriormente no tutorial).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

Lista suspensa "Estrutura de teste" na caixa de diálogo acima é preenchida com todos os disponíveis ASP.NET MVC unidade teste modelos de projeto instalados no computador. As versões podem ser baixadas para o NUnit, MBUnit e XUnit. Também há suporte para a estrutura interna do teste de unidade do Visual Studio.

*Observação: O Visual Studio Unit Test Framework só está disponível com o Visual Studio 2008 Professional e versões superiores. Se você estiver usando o VS 2008 Standard Edition ou o Visual Web Developer 2008 Express, você precisará baixar e instalar as extensões do NUnit, o MBUnit ou XUnit para o ASP.NET MVC para que essa caixa de diálogo a ser mostrado. A caixa de diálogo não será exibida se não existem quaisquer estruturas de teste instaladas.*

Vamos usar o nome de "NerdDinner.Tests" padrão para o projeto de teste que criamos e use a opção de framework "Visual Studio de teste de unidade". Quando clicamos no botão "okey" Visual Studio criará uma solução para nós com dois projetos nela: um para nosso aplicativo web e outro para nossos testes de unidade:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Examinando a estrutura de diretório do NerdDinner

Quando você cria um novo aplicativo ASP.NET MVC com o Visual Studio, ele adiciona automaticamente um número de arquivos e diretórios para o projeto:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Projetos do ASP.NET MVC por padrão tem seis diretórios de nível superior:

| **Diretório** | **Finalidade** |
| --- | --- |
| **/ Controladores** | Onde você coloca a classes do controlador que manipulam as solicitações de URL |
| **Ou os modelos** | Onde você coloca as classes que representam e manipulam dados |
| **/ Modos de exibição** | Onde você coloca arquivos de modelo de interface do usuário que são responsáveis por saída de renderização |
| **/ Scripts** | Onde você coloca arquivos de biblioteca de JavaScript e scripts (. js) |
| **/Content** | Onde você colocou o CSS e arquivos de imagem e outros tipos de conteúdo não-dinâmico/não-JavaScript |
| **/ Aplicativo\_dados** | Onde você armazena arquivos de dados ser leitura/gravação. |

ASP.NET MVC não exige essa estrutura. Na verdade, os desenvolvedores que trabalham em aplicativos grandes normalmente particionará o aplicativo de backup em vários projetos para torná-lo mais fácil de gerenciar (por exemplo: classes de modelo de dados geralmente entram em um projeto de biblioteca de classe separada do aplicativo web). No entanto, a estrutura de projeto padrão, forneça uma convenção de diretório padrão legal que podemos usar para manter as nossas preocupações de aplicativo limpa.

Quando podemos expandir o diretório /Controllers vou encontrar o Visual Studio adicionou duas classes de controlador – HomeController e AccountController – por padrão ao projeto:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Quando podemos expandir o diretório /Views, podemos encontrará três subdiretórios – /Home, /Account e /Shared –, bem como modelo de vários arquivos nelas também foram adicionados ao projeto por padrão:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Quando expandirmos a /Content e diretórios de /Scripts, vou encontrar o suportem a um arquivo CSS que é usado para definir o estilo de todos os HTML no site, bem como bibliotecas JavaScript que podem habilitar o ASP.NET AJAX e jQuery dentro do aplicativo:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Quando podemos expandir o projeto NerdDinner.Tests vou encontrar duas classes que contêm testes de unidade para nossas classes de controlador:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Esses arquivos padrão adicionados pelo Visual Studio nos fornecem uma estrutura básica para um aplicativo de trabalho - completo com a página inicial, sobre a página, páginas de logon/logoff/registro de conta e uma página de erro sem tratamento (tudo conectada e trabalhando fora da caixa).

### <a name="running-the-nerddinner-application"></a>Execução do aplicativo NerdDinner

Podemos executar o projeto, escolhendo qualquer uma de **Debug -&gt;iniciar depuração** ou **Debug -&gt;Start Without Debugging** itens de menu:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Isso iniciará o servidor Web ASP.NET interno-que vem com o Visual Studio e executar nosso aplicativo:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Abaixo está a home page do nosso novo projeto (URL: "/") quando ele é executado:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Clique na guia "Sobre" exibe uma página sobre (URL: "/ Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Clicando no link "Fazer logon" na parte superior direita nos leva a uma página de logon (URL: "/ conta/LogOn")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Se não há uma conta de logon, podemos clicar no link de registro (URL: "/ conta/registro") para criar um:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

O código para implementar a casa acima, sobre e o logoff / registrar funcionalidade foi adicionada por padrão, quando criamos nosso novo projeto. Vamos usá-lo como ponto de partida do nosso aplicativo.

### <a name="testing-the-nerddinner-application"></a>Testando o aplicativo NerdDinner

Se estivermos utilizando a Professional Edition ou uma versão superior do Visual Studio 2008, podemos usar o suporte de IDE do Visual Studio de teste de unidade interna para o projeto de teste:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Escolhendo uma das opções acima, abra o painel de "resultados de teste" dentro do IDE e forneça status de aprovação/reprovação nos testes de 27 unidade incluído em nosso novo projeto que abrangem a funcionalidade interna:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Posteriormente no tutorial vamos falar mais sobre os testes automatizados e adicionar testes de unidade adicionais que abrangem a funcionalidade do aplicativo que podemos implementar.

### <a name="next-step"></a>Próxima etapa

Agora temos uma estrutura de aplicativo básico em vigor. Agora vamos [criar um banco de dados para armazenar nossos dados de aplicativo](create-a-database.md).

> [!div class="step-by-step"]
> [Anterior](introducing-the-nerddinner-tutorial.md)
> [Próximo](create-a-database.md)
