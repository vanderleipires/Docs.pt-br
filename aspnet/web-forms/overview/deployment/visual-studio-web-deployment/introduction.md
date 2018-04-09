---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Implantação de Web do ASP.NET usando o Visual Studio: Introdução | Microsoft Docs'
author: tdykstra
description: Esta série de tutorial mostra como implantar um ASP.NET (publicar) aplicativo da web para aplicativos de Web do serviço de aplicativo do Azure ou um provedor de hospedagem de terceiros pelo usando V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 1ff4cc3b0fa6ce7e6cdc833a0c2f7fea2050c4e6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Implantação de Web do ASP.NET usando o Visual Studio: Introdução
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutorial mostra como implantar um ASP.NET (publicar) aplicativo da web para aplicativos de Web do serviço de aplicativo do Azure ou um provedor de hospedagem de terceiros pelo usando o Visual Studio 2012 com o SDK do Azure para .NET. A maioria dos procedimentos é semelhante para Visual Studio 2013.
> 
> Você desenvolver um aplicativo web para disponibilizá-lo para as pessoas na Internet. Mas tutoriais de programação da web geralmente pare logo após eles mostramos como obter algo trabalhando em seu computador de desenvolvimento. Esta série de tutoriais começa onde outros deixam desativados: você tiver criado um aplicativo web, testados, e está pronto para começar. O que vem a seguir? Esses tutoriais mostram como implantar primeiro para o IIS no computador de desenvolvimento local para teste e, em seguida, no Azure ou em um provedor de hospedagem de terceiros para preparação e produção. O aplicativo de exemplo que você implantará é um projeto de aplicativo web que usa o Entity Framework, SQL Server e o sistema de associação do ASP.NET. O aplicativo de exemplo usa o Web Forms do ASP.NET, mas também aplicam os procedimentos mostrados para ASP.NET MVC e a API da Web.
> 
> Estes tutoriais presumem que você sabe como trabalhar com o ASP.NET no Visual Studio. Se você não fizer isso, um bom lugar para começar é um [básico ASP.NET Web Forms Tutorial](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) ou um [basic ASP.NET MVC Tutorial](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [Fórum de implantação do ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) ou [StackOverflow](http://stackoverflow.com).
> 
> Este conteúdo também está disponível como um e-book gratuito no [na Galeria do TechNet E-Book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Visão Geral

Esses tutoriais orientam você na implantação de um aplicativo web ASP.NET que inclui bancos de dados do SQL Server. Você implantará primeiro para o IIS no computador de desenvolvimento local para teste e, em seguida, para aplicativos Web no serviço de aplicativo do Azure e banco de dados SQL Azure para preparação e produção. Você verá como implantar usando o Visual Studio, um clique para publicar, e você verá como implantar usando a linha de comando.

O número de tutoriais pode fazer com que o processo de implantação parecer desanimador. Na verdade, os procedimentos básicos são simples. No entanto, em situações reais, muitas vezes é preciso realizar tarefas de implantação adicionais — por exemplo, definindo permissões de pasta no servidor de destino. Mostrado algumas dessas tarefas adicionais, na esperança de que os tutoriais não se esqueça de informações que podem impedir que você implantar um aplicativo real.

Os tutoriais são projetados para funcionar em sequência, e cada parte baseia-se na parte anterior. Você pode ignorar partes que não são relevantes para sua situação, mas, em seguida, talvez você precise ajustar os procedimentos em tutoriais subsequentes.

## <a name="intended-audience"></a>Público-alvo

Os tutoriais são destinados a desenvolvedores do ASP.NET que trabalham em ambientes onde:

- O ambiente de produção é um provedor de hospedagem de terceiros ou de aplicativos de Web do serviço de aplicativo do Azure.
- Implantação não está limitada a um processo de integração contínua, mas pode ser feita diretamente do Visual Studio.

Implantação de [controle de origem](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) usando um [fornecimento contínuo](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) processo não é abordado esses tutoriais, exceto um tutorial que mostra como implantar a partir da linha de comando. Para obter informações sobre a entrega contínua, consulte os seguintes recursos:

- [Integração contínua e fornecimento contínuo (Criando aplicativos de nuvem do mundo Real com o Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Implantar um aplicativo web no serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Implantando aplicativos da Web em cenários corporativos](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (um conjunto mais antigo de tutoriais escritos para o Visual Studio 2010, que ainda tem informações úteis para ambientes corporativos.)

## <a name="using-a-third-party-hosting-provider"></a>Usando um provedor de hospedagem de terceiros

Os tutoriais levá-lo pelo processo de configuração de uma conta do Azure e implantar o aplicativo para aplicativos Web no serviço de aplicativo do Azure para preparação e produção. No entanto, você pode usar os mesmos procedimentos básicos para implantar em um provedor de hospedagem de terceiros de sua escolha. Onde ir os tutoriais sobre processos exclusivos para o Azure, eles explicam que e informar quais diferenças que você pode esperar em um provedor de hospedagem de terceiros.

## <a name="deploying-web-app-projects"></a>Implantando projetos de aplicativo web

O aplicativo de exemplo que você baixa e implantar para esses tutoriais é um projeto de aplicativo de web do Visual Studio. No entanto, se você instalar a versão mais recente [atualização de publicar Web do Visual Studio](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), você pode usar as ferramentas e os mesmos métodos de implantação para projetos de aplicativo web.

## <a name="deploying-aspnet-mvc-projects"></a>Implantando projetos do ASP.NET MVC

O aplicativo de exemplo é um projeto Web Forms do ASP.NET, mas tudo o que você saiba como fazer também é aplicável ao ASP.NET MVC. Um projeto MVC do Visual Studio é apenas outra forma de projeto de aplicativo web. A única diferença é que se você estiver implantando em um provedor de hospedagem que não dão suporte ao ASP.NET MVC ou sua versão de destino do mesmo, você deve verificar se que você tenha instalado o apropriada ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) ou [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) pacote NuGet em seu projeto.

## <a name="programming-language"></a>Linguagem de programação

O aplicativo de exemplo usa c#, mas os tutoriais não exigem o conhecimento do c# e as técnicas de implantação mostradas nos tutoriais não são específicas do idioma.

## <a name="database-deployment-methods"></a>Métodos de implantação de banco de dados

Há três maneiras que você pode implantar um banco de dados do SQL Server junto com a implantação da web no Visual Studio:

- Migrações do Entity Framework Code First
- O provedor de implantação da Web dbDacFx
- O provedor de implantação da Web dbFullSql

Neste tutorial, você usará os dois primeiros desses métodos. O provedor de implantação da Web dbFullSql é um método herdado que não é mais recomendado exceto para alguns cenários específicos, como de migração do SQL Server Compact para o SQL Server.

Os métodos mostrados neste tutorial são bancos de dados do SQL Server, não o SQL Server Compact. Para obter informações sobre como implantar um banco de dados do SQL Server Compact, consulte [Visual Studio Web implantação com o SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Os métodos mostrados neste tutorial exigem que você use a implantação da Web método de publicação. Se você preferir publicar outro método, como FTP, sistema de arquivos ou FPSE, consulte [Implantando um banco de dados separadamente de implantação de aplicativos web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) do mapa de conteúdo de implantação da Web do Visual Studio e ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>Migrações do Entity Framework Code First

O Entity Framework versão 4.3, a Microsoft introduziu migrações do Code First. Migrações do Code First automatiza o processo de fazer alterações incrementais para um modelo de dados e propagar essas alterações para o banco de dados. Em versões anteriores do Code First, você normalmente permitem que o Entity Framework descartar e recriar o banco de dados cada vez que você alterar o modelo de dados. Isso não é um problema no desenvolvimento porque os dados de teste são facilmente recriados, mas em produção, você geralmente deseja atualizar o esquema de banco de dados sem descartar o banco de dados. O recurso de migrações permite Code First atualizar o banco de dados sem descartar e recriá-lo. Você pode deixar Code First automaticamente decidir como fazer as alterações de esquema necessárias, ou você pode escrever código que personaliza as alterações. Para obter uma introdução para migrações do Code First, consulte [migrações do Code First](https://msdn.microsoft.com/library/hh770484.aspx).

Quando você estiver implantando um projeto da web, o Visual Studio pode automatizar o processo de implantação de um banco de dados que é gerenciado pelo migrações do Code First. Quando você cria o perfil de publicação, você pode selecionar uma caixa de seleção rotulada executar migrações do Code First (executado na inicialização do aplicativo). Essa configuração faz com que o processo de implantação configurar automaticamente o arquivo de Web. config do aplicativo no servidor de destino para que use o Code First a `MigrateDatabaseToLatestVersion` classe inicializador.

O Visual Studio não faz nada com o banco de dados durante o processo de implantação. Quando um aplicativo implantado acessa o banco de dados pela primeira vez após a implantação, Code First automaticamente cria o banco de dados ou atualiza o esquema de banco de dados para a versão mais recente. Se o aplicativo implementa um método de propagação de migrações, o método é executado depois que o banco de dados é criado ou o esquema é atualizado.

Neste tutorial, você usará migrações do Code First para implantar o banco de dados do aplicativo.

### <a name="the-dbdacfx-web-deploy-provider"></a>O provedor de implantação da Web dbDacFx

Para um banco de dados do SQL Server que não é gerenciado pelo Entity Framework Code First, você pode selecionar uma caixa de seleção rotulada Atualizar banco de dados quando você configurar o perfil de publicação. Durante a implantação inicial, o provedor dbDacFx cria tabelas e outros objetos de banco de dados no banco de dados de destino para coincidir com o banco de dados de origem. Em implantações subsequentes, o provedor determina qual é a diferença entre os bancos de dados de origem e de destino e atualiza o esquema do banco de dados de destino para coincidir com o banco de dados de origem. Por padrão, o provedor não faça as alterações que causam a perda de dados, como quando uma tabela ou coluna é descartada.

Esse método não automatizar a implantação de dados em tabelas de banco de dados, mas você pode criar scripts para fazer isso e configurar o Visual Studio para executá-los durante a implantação. Outro motivo para executar scripts durante a implantação é para fazer alterações de esquema que não podem ser feitas automaticamente porque poderia causar perda de dados.

Neste tutorial, você usará o provedor dbDacFx para implantar o banco de dados de associação do ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Solução de problemas durante este tutorial

Quando ocorre um erro durante a implantação, ou se o site implantado não executar corretamente, as mensagens de erro sempre não fornecerem uma solução óbvia. Para ajudá-lo com alguns cenários comuns de problema, um [página de referência de solução](troubleshooting.md) está disponível. Se você receber uma mensagem de erro ou algo não funciona ao percorrer os tutoriais, certifique-se de verificar a página de solução de problemas.

## <a name="comments-welcome"></a>Comentários de boas-vindas

Comentários sobre os tutoriais são boas-vindas, e quando o tutorial é atualizado todos os esforços serão feito para correções de conta ou sugestões de melhorias que são fornecidas no tutoriais comentários.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial foi escrito para os seguintes produtos:

- Windows 8 ou Windows 7.
- Visual Studio 2012 ou Visual Studio 2012 Express para Web com [a atualização mais recente](https://go.microsoft.com/fwlink/?LinkId=272486).
- [SDK do Azure para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Você pode seguir o tutorial usando o Visual Studio 2010 SP1 ou Visual Studio 2013, mas algumas capturas de tela será diferentes e alguns recursos serão diferentes.

Se você estiver usando o Visual Studio 2013, instale o [SDK do Azure para Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Se você estiver usando o Visual Studio 2010 SP1, instale o software a seguir:

- [SDK do Azure para Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx).

Dependendo de quantos das dependências do SDK você já tem em seu computador, a instalação do SDK do Azure pode levar muito tempo, de alguns minutos a meia hora ou mais. É necessário o SDK do Azure, mesmo se você pretende publicar em um provedor de hospedagem de terceiros em vez de para o Azure, como o SDK inclui as atualizações mais recentes da web do Visual Studio publicar recursos.

> [!NOTE]
> Este tutorial foi escrito com versão 1.8.1 do SDK do Azure. Versões mais recentes com recursos adicionais foram lançadas desde então. Os tutoriais foram atualizados para mencionar esses recursos e links para recursos que têm mais informações sobre eles.


As instruções e capturas de tela são baseadas no Windows 8, mas os tutoriais explicam as diferenças para o Windows 7.

Algum outro software é necessário para concluir o tutorial, mas você não precisa ter instalado ainda. O tutorial orientará você durante as etapas para instalá-lo quando necessário.

## <a name="download-the-sample-application"></a>Baixe o aplicativo de exemplo

O aplicativo que você implantará é nomeado Contoso University e já foi criado para você. É uma versão simplificada de um site de university, livremente baseado no aplicativo Contoso University descrito no [tutoriais do Entity Framework no site do ASP.NET](https://asp.net/entity-framework/tutorials).

Quando você tiver os pré-requisitos instalados, baixe o [aplicativo Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). O *. zip* arquivo contém várias versões do projeto. Para trabalhar com as etapas do tutorial, inicie com o projeto localizado na pasta c#. Para ver a aparência de projeto no final dos tutoriais, abra o projeto na pasta ContosoUniversity-End.

Para preparar o projeto para realizar as etapas do tutoriais, execute as seguintes etapas:

1. Salve os arquivos de solução de ContosoUniversity da pasta c# em uma pasta chamada ContosoUniversity em qualquer pasta que você pode usar para trabalhar com projetos do Visual Studio.

    Por padrão, essa é a pasta a seguir para o Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Para as capturas de tela neste tutorial, a pasta do projeto está localizada no diretório raiz no `C`: unidade.)
2. Inicie o Visual Studio e abra o projeto.
3. Em **Solution Explorer**, clique com botão direito a solução e clique em **EnableNuGet a restauração de pacotes**.
4. Compile a solução.
5. Se você obtiver erros de compilação, restaure manualmente os pacotes do NuGet:

    1. Em **Solution Explorer**, clique com botão direito a solução e, em seguida, clique em **gerenciar pacotes NuGet para solução**.
    2. Na parte superior do **gerenciar pacotes NuGet** caixa de diálogo, você verá **pacotes do NuGet alguns estão presentes nesta solução. Clique para restaurar.** Clique o **restaurar** botão.
    3. Recompile a solução.
6. Pressione CTRL-F5 para executar o aplicativo.

    O aplicativo abre a home page do Contoso University.

    ![Home Page Dev](introduction/_static/image1.png)

    (Pode haver um tempo de espera enquanto o Visual Studio inicia a instância do SQL Server Express LocalDB, e você poderá obter um erro de tempo limite se que o processo leva muito tempo. Nesse caso apenas inicie o projeto novamente.)

As páginas podem ser acessadas na barra de menus e permitem que você execute as seguintes funções:

- Exibir estatísticas de aluno (a página sobre).
- Exibir, editar, excluir e adicionar os alunos.
- Exibir e editar os cursos.
- Exibir e editar professores.
- Exibir e editar departamentos.

Seguem as capturas de tela de algumas páginas representativas.

![Desenvolvimento de página de alunos](introduction/_static/image2.png)

![Adicionar os alunos desenvolvimento de página](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Analise os recursos do aplicativo que afetam a implantação

Os seguintes recursos do aplicativo afetam como implantar ou o que você precisa fazer para implantá-lo. Cada um deles é explicada em mais detalhes os tutoriais a seguir na série.

- Universidade de Contoso usa um banco de dados do SQL Server para armazenar dados de aplicativo, como nomes de student e instrutor. O banco de dados contiver uma mistura de dados de teste e produção e, quando você implanta em produção, você precisa excluir os dados de teste.
- O aplicativo usa o sistema de associação do ASP.NET, que armazena informações de conta de usuário em um banco de dados do SQL Server. O aplicativo define um usuário de administrador que tem acesso a algumas informações restritas. Você precisa implantar o banco de dados de associação sem contas de teste, mas com uma conta de administrador.
- O aplicativo usa um utilitário de relatório e log de erros do terceiro. Esse utilitário é fornecido em um assembly que deve ser implantado com o aplicativo.
- O utilitário de log de erro grava informações de erro em arquivos XML para uma pasta de arquivos. Você precisa certificar-se de que a conta que o ASP.NET é executado no site implantado tem permissão de gravação nessa pasta, e você precisa excluir esta pasta de implantação. (Caso contrário, os dados de log de erros do ambiente de teste podem ser implantados para produção e/ou arquivos de log de erros de produção podem ser excluídos.)
- O aplicativo inclui algumas configurações que devem ser alteradas no implantado *Web. config* arquivo dependendo do ambiente de destino (teste, preparo ou produção) e outras configurações que devem ser alteradas dependendo da compilação configuração (Debug ou Release).
- A solução do Visual Studio inclui um projeto de biblioteca de classe. Somente o assembly que gera este projeto deve ser implantado, não o próprio projeto.

## <a name="summary"></a>Resumo

Neste tutorial primeiro na série, você baixou o projeto do Visual Studio de exemplo e revisar os recursos de site que afetam como implantar o aplicativo. Os tutoriais a seguir, você preparar para implantação, configurando a algumas dessas coisas a serem manipulados automaticamente. Outras pessoas que você se encarrega manualmente.

> [!div class="step-by-step"]
> [Avançar](preparing-databases.md)
