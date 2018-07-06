---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio: Introdução - 1 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 7449f16367b1eddd7ed66544792129d2a3bc9627
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806064"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio: Introdução - 1 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se você instalar a atualização de publicação na Web.
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server que não seja o SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Esses tutoriais orientá-lo Implantando primeiro ao IIS no seu computador de desenvolvimento local para teste e, em seguida, em um provedor de hospedagem de terceiros. O aplicativo que você implantará usa um banco de dados do aplicativo e um banco de dados de associação do ASP.NET. Você começa usando o SQL Server Compact e a implantação para o SQL Server Compact e tutoriais posteriores mostram como implantar as alterações do banco de dados e como migrar para o SQL Server.
> 
> Os tutoriais presumem que você sabe como trabalhar com o ASP.NET no Visual Studio. Se você não fizer isso, um bom lugar para começar é um [básica Tutorial do ASP.NET Web Forms](../tailspin-spyworks/tailspin-spyworks-part-1.md) ou um [Tutorial básico do ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Se você tiver perguntas que não estão diretamente relacionadas para o tutorial, você pode postá-los para o [Fórum de implantação do ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Visão geral

Esses tutoriais orientá-lo Implantando primeiro ao IIS no seu computador de desenvolvimento local para teste e, em seguida, em um provedor de hospedagem de terceiros. O aplicativo que você implantará usa um banco de dados do aplicativo e um banco de dados de associação do ASP.NET. Você começa usando o SQL Server Compact e a implantação para o SQL Server Compact e tutoriais posteriores mostram como implantar as alterações do banco de dados e como migrar para o SQL Server.

O número de tutoriais – 11 em todas as mais de uma página de solução de problemas pode tornar o processo de implantação parecer assustadora. Na verdade, os procedimentos básicos para implantar um site compõem uma parte relativamente pequena do conjunto de tutoriais. No entanto, em situações do mundo real, geralmente precisar de informações sobre algum aspecto extra pequena, mas importante da implantação — por exemplo, definindo permissões de pasta no servidor de destino. Há muitas dessas técnicas adicionais nos tutoriais, na esperança de que os tutoriais de não deixar as informações que podem impedir que você implantar um aplicativo real.

Os tutoriais são projetados para executar em sequência, e cada parte se baseia na parte anterior. No entanto, você poderá ignorar as partes que não são relevantes para sua situação. (Ignorar partes pode exigir que você ajustar os procedimentos em tutoriais posteriores.)

## <a name="intended-audience"></a>Público-alvo

Os tutoriais são destinados a desenvolvedores do ASP.NET que trabalham em organizações de pequenas porte ou em outros ambientes em que:

- Um processo de integração contínua (compilações automatizadas e implantação) não é usado.
- O ambiente de produção é um provedor de hospedagem de terceiros.
- Uma pessoa normalmente executa várias funções (a mesma pessoa desenvolve, testa e implanta).

Em ambientes empresariais, é mais comum para implementar processos de integração contínua e o ambiente de produção é geralmente hospedado pelos servidores da empresa. Pessoas diferentes também geralmente executam funções diferentes. Para obter informações sobre implantação corporativa, consulte [implantação de aplicativos Web em cenários empresariais](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

As organizações de todos os tamanhos também podem implantar aplicativos web no Azure e a maioria dos procedimentos mostradas nos tutoriais também se aplicam a aplicativos de Web de serviços de aplicativo do Azure. Para obter uma introdução ao Azure, consulte [ https://azure.microsoft.com ](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>O provedor de hospedagem mostrado nos tutoriais

Os tutoriais guiarão você pelo processo de configuração de uma conta com uma empresa de hospedagem e implantação do aplicativo para esse provedor de hospedagem. Uma empresa de hospedagem específica foi escolhida para que os tutoriais poderiam ilustram a experiência completa de implantação em um site ao vivo. Cada empresa de hospedagem fornece recursos diferentes, e a experiência de implantação para seus servidores varia um pouco; No entanto, o processo descrito neste tutorial é típico para o processo geral.

O provedor de hospedagem usado para este tutorial, Cytanium.com, é um dos muitos que estão disponíveis e seu uso neste tutorial não constitui um endosso ou recomendação.

## <a name="deploying-web-site-projects"></a>Implantação de projetos de Site da Web

Contoso University é um projeto de aplicativo web Visual Studio. Não se aplicam à maioria dos métodos de implantação e ferramentas demonstradas neste tutorial [projetos de Site](https://msdn.microsoft.com/library/dd547590.aspx). Para obter informações sobre como implantar projetos de site, consulte [mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Implantação de projetos do ASP.NET MVC

Para este tutorial, você implanta um projeto de Web Forms do ASP.NET, mas tudo o que você aprenderá a fazer é aplicável ao ASP.NET MVC também. Um projeto do MVC do Visual Studio é apenas outra forma de projeto de aplicativo web. A única diferença é que se você estiver implantando em um provedor de hospedagem que não dão suporte ao ASP.NET MVC ou sua versão de destino dele, certifique-se de que você tenha instalado o apropriada ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) ou [MVC 4](http://nuget.org/packages/aspnetmvc)) O pacote NuGet em seu projeto.

## <a name="programming-language"></a>Linguagem de programação

O aplicativo de exemplo usa c#, mas os tutoriais não exigem conhecimento do c# e mostradas nos tutoriais de técnicas de implantação não são específicas do idioma.

## <a name="troubleshooting-during-this-tutorial"></a>Solução de problemas durante este Tutorial

Quando ocorrer um erro durante a implantação, ou se o site implantado não é executado corretamente, as mensagens de erro sempre não fornecem uma solução. Para ajudá-lo com alguns cenários comuns de problema, uma [página de referência de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) está disponível. Se você receber uma mensagem de erro ou se algo não funciona ao percorrer os tutoriais, certifique-se de verificar a página de solução de problemas.

## <a name="comments-welcome"></a>Bem-vindo de comentários

Comentários sobre os tutoriais são boas-vindos e quando o tutorial será atualizado todos os esforços serão feito para correções de conta ou sugestões de melhorias que são fornecidas nos comentários do tutoriais.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que você tenha o Windows 7 ou posterior e um dos seguintes produtos instalados no seu computador:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Se você tiver o Visual Studio 2010 SP1 ou Visual Web Developer Express 2010 SP1, instale os seguintes produtos também:

- [SDK do Azure para .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (inclui a atualização de publicação na Web)
- [Microsoft Visual Studio 2010 SP1 Tools para SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Qualquer outro software é necessário para concluir o tutorial, mas você não precisa ter que carregado ainda. O tutorial o orientará pelas etapas para instalá-lo quando necessário.

## <a name="downloading-the-sample-application"></a>Baixar o aplicativo de exemplo

O aplicativo que você vai implantar é denominado Contoso University e já foi criado para você. É uma versão simplificada de um site de Universidade, vagamente baseado no aplicativo Contoso University descrito na [tutoriais do Entity Framework no site do ASP.NET](https://asp.net/entity-framework/tutorials).

Quando você tem os pré-requisitos instalados, baixe o [aplicativo web Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). O *. zip* arquivo contém várias versões do projeto e um arquivo PDF que contém todos os tutoriais de 12. Para trabalhar com as etapas do tutorial, comece com Begin ContosoUniversity. Para ver a aparência de projeto no final dos tutoriais, abra ContosoUniversity-End. Para ver a aparência de projeto antes da migração para o SQL Server completo Tutorial 10, abra AfterTutorial09 ContosoUniversity.

Para preparar a percorrer as etapas do tutorial, início do salvamento ContosoUniversity-até a pasta em que você pode usar para trabalhar com projetos do Visual Studio. Por padrão, essa é a pasta a seguir:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Para as capturas de tela neste tutorial, a pasta do projeto está localizada no diretório raiz no `C`: drive.)

Inicie o Visual Studio, abra o projeto e pressione CTRL-F5 para executá-lo.

[![Home_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

As páginas do site são acessíveis na barra de menus e permitem que você realize as seguintes funções:

- Exibir estatísticas de alunos (a página sobre).
- Exibir, editar, excluir e adicionar alunos.
- Exibir e editar cursos.
- Exibir e editar instrutores.
- Exibir e editar os departamentos.

A seguir estão as capturas de tela de algumas páginas representativas.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Revisar recursos de aplicativos que afetam a implantação

Os seguintes recursos do aplicativo afetam como implantá-lo ou o que você precisa fazer para implantá-lo. Cada um deles é explicada mais detalhadamente nos tutoriais a seguir na série.

- Contoso University usa um banco de dados do SQL Server Compact para armazenar dados de aplicativo, como nomes de aluno e instrutor. O banco de dados contiver uma mistura de dados de teste e dados de produção e, quando você implanta em produção, você precisará excluir os dados de teste. Posteriormente na série de tutoriais, você vai migrar do SQL Server Compact para o SQL Server.
- O aplicativo usa o sistema de associação do ASP.NET, que armazena informações de conta de usuário em um banco de dados do SQL Server Compact. O aplicativo define um usuário de administrador que tenha acesso a algumas informações restritas. Você precisará implantar o banco de dados de associação sem contas de teste, mas com uma conta de administrador.
- Como o banco de dados do aplicativo e o banco de dados de associação usam SQL Server Compact como o mecanismo de banco de dados, você precisa implantar o mecanismo de banco de dados para o provedor de hospedagem, bem como os bancos de dados em si.
- O aplicativo usa provedores de universal de associação ASP.NET para que o sistema de associação pode armazenar seus dados em um banco de dados do SQL Server Compact. O assembly que contém os provedores de associação universal deve ser implantado com o aplicativo.
- O aplicativo usa o Entity Framework 5.0 para acessar dados no banco de dados do aplicativo. O assembly que contém o Entity Framework 5.0 deve ser implantado com o aplicativo.
- O aplicativo usa o utilitário de relatório e registro em log um erro de terceiros. Esse utilitário é fornecido em um assembly que deve ser implantado com o aplicativo.
- O utilitário de registro em log de erro grava informações de erro em arquivos XML para uma pasta de arquivos. Você precisa certificar-se de que a conta que o ASP.NET é executado no site implantado tem permissão de gravação nessa pasta, e você precisará excluir essa pasta de implantação. (Caso contrário, os dados de log de erros do ambiente de teste podem ser implantados em produção e/ou arquivos de log de erros de produção podem ter sido excluídos.)
- O aplicativo inclui algumas configurações que devem ser alteradas em implantado *Web. config* arquivo dependendo do ambiente de destino (teste ou produção) e outras configurações que devem ser alteradas dependendo da compilação configuração (Debug ou Release).
- A solução do Visual Studio inclui um projeto de biblioteca de classes. Somente o assembly que gera este projeto deve ser implantado, não o próprio projeto.

Neste tutorial primeiro da série, você baixou o projeto do Visual Studio e revisado recursos de site que afetam como você pode implantar o aplicativo. Nos tutoriais a seguir, você prepara para implantação, configurando a algumas dessas coisas a serem manipulados automaticamente. Outras pessoas que você cuidar manualmente.

> [!div class="step-by-step"]
> [Avançar](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
