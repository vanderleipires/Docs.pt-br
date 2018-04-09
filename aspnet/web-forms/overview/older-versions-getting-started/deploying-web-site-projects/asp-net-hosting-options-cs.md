---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: ASP.NET hospedagem opções (c#) | Microsoft Docs
author: rick-anderson
description: Aplicativos web ASP.NET são normalmente criados, criado e testado em um ambiente de desenvolvimento local e precisam ser implantados em um o ambiente de produção...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 6f8bb0e5a34d84d448af56285e8761c447229f7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-hosting-options-c"></a>Opções de hospedagem ASP.NET (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> Aplicativos web ASP.NET normalmente são criados, criados e testados em um ambiente de desenvolvimento local e precisam ser implantados em um ambiente de produção, quando estiver pronto para liberação. Este tutorial fornece uma visão geral do processo de implantação e serve como uma introdução para esta série de tutoriais.


## <a name="introduction"></a>Introdução

Aplicativos da Web geralmente são criados, criados e testados em um ambiente de desenvolvimento que é acessível somente para os programadores trabalhando no site. Depois que o aplicativo está pronto para ser liberado, ele é movido para um ambiente de produção em que o site pode ser acessado por qualquer pessoa na Internet. Esse processo de implantação apresenta uma série de desafios:

- Um ambiente de produção deve existir e estar configurado corretamente, antes de um aplicativo ASP.NET pode ser implantado; Além disso, o ambiente de produção deve ser mantido atualizado com os patches de segurança mais recentes.
- O conjunto correto de arquivos de marcação, arquivos de código e arquivos de suporte deve ser copiado no ambiente de desenvolvimento para o ambiente de produção. Para aplicativos controlados por dados, isso pode exigir copiando o esquema de banco de dados e/ou dados.
- Pode haver diferenças de configuração entre os dois ambientes. O servidor banco de dados de email ou de cadeia de caracteres de conexão usado no ambiente de desenvolvimento provavelmente será diferente do ambiente de produção. Além disso, o comportamento do aplicativo pode depender do ambiente. Por exemplo, quando ocorre um erro no desenvolvimento de detalhes do erro podem ser exibidas na tela, mas quando ocorre um erro em produção, uma página de erro amigável deve ser exibida em vez disso, e os detalhes do erro enviado por email para os desenvolvedores.

Para eliminar o primeiro desafio - configurar e manter um ambiente de produção - muitos usuários e empresas terceirizem a seus ambientes de produção para *web provedores de hospedagem*. Um provedor de hospedagem na web é uma empresa que gerencia o ambiente de produção em seu nome. Há inúmeras web provedores de host, cada um com preços diferentes e níveis de serviço; Consulte a seção "Encontrar um provedor de Host da Web" para obter dicas sobre como localizar esse provedor de serviço.

Este é o primeiro em uma série de tutoriais que são as etapas envolvidas na implantação de um aplicativo da web ASP.NET para um ambiente de produção gerenciado por um provedor de host da web. Ao longo destes tutoriais vamos examinar:

- Os arquivos que precisam ser implantados para o provedor de host da web.
- Ferramentas para simplificar o processo de implantação.
- Como implantar um banco de dados.
- Dicas para implantação de um banco de dados que usa [o provedor de associação com base em SQL e funções](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), juntamente com maneiras para simular a ferramenta de administração de site em um ambiente de produção.
- Estratégias para a atualização suave do banco de dados em produção com as alterações feitas durante o desenvolvimento.
- Técnicas para o log de erros que ocorrem em produção e maneiras de notificar os desenvolvedores quando ocorre um erro.

Esses tutoriais são direcionados para ser conciso e fornecem instruções passo a passo com capturas de tela suficiente para orientá-lo pelo processo visualmente. Este tutorial inaugural fornece uma visão geral do processo de implantação do ASP.NET e conselhos sobre como localizar um provedor de hospedagem na web. Vamos começar!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Uma visão geral do processo de implantação do ASP.NET

Em resumo, implantar um aplicativo ASP.NET envolve três etapas a seguir:

1. Configure o aplicativo web, o servidor web e o banco de dados no ambiente de produção.
2. Sincronizar as páginas ASP.NET, arquivos de código, os assemblies no `Bin` pastas e arquivos de suporte de HTML como arquivos CSS e JavaScript.
3. Sincronize o esquema de banco de dados e/ou dados.

As informações de configuração para um aplicativo da web geralmente estão localizadas no `Web.config` de arquivo e inclui cadeias de caracteres de conexão de banco de dados, critérios de tratamento de erros, regras e informações do servidor de email de regravação de URL. Muitas vezes, essa informação é diferente de um aplicativo em desenvolvimento em comparação com o mesmo aplicativo em produção. Por exemplo, ao desenvolver um aplicativo é melhor usar um banco de dados de desenvolvimento, de forma que você está testando não no banco de dados de produção. Como resultado, as cadeias de caracteres de conexão do banco de dados geralmente são diferentes entre os aplicativos de desenvolvimento e produção. Devido a essas diferenças, parte da implantação envolve alterações às informações de configuração do aplicativo da web.

Além das alterações de configuração do aplicativo web, etapa 1 também pode envolver a configuração para o servidor web e o banco de dados. Por exemplo, se uma página ASP.NET cria ou exclui arquivos de um diretório no servidor web, em seguida, o servidor web precisa ser configurado para permitir que essas modificações no sistema de arquivos. Da mesma forma, pode haver configurações de permissão ou de autenticação que precisam ser feitas no banco de dados.


Etapa 2 envolve sincronizando o conjunto de páginas do ASP.NET essenciais e arquivos de suporte entre os ambientes de desenvolvimento e produção. O conjunto específico de ASP. Arquivos relacionados à rede que precisam ser sincronizados entre os dois ambientes depende do tipo de projeto criado no Visual Studio e é a discussão no tutorial de Avançar, [ *determinar quais arquivos precisam ser implantados*](determining-what-files-need-to-be-deployed-cs.md). Os tutoriais de terceiros e quarto - [ *Implantando seu Site usando FTP* ](deploying-your-site-using-an-ftp-client-cs.md) e [ *implantando seu Site usando o Visual Studio* ](deploying-your-site-using-visual-studio-cs.md) -examinar diferentes ferramentas e técnicas para esses arquivos de sincronização.

Ao criar aplicativos orientados a dados, há geralmente dois bancos de dados que está sendo usados: uma para desenvolvimento e outro na produção. Durante o desenvolvimento, o esquema do banco de dados de desenvolvimento pode ser modificada para incluir novas tabelas, colunas, procedimentos armazenados e gatilhos ou pode ser modificada para remover ou renomear objetos de banco de dados existente. Entre a hora em que essas alterações são feitas e a hora em que o aplicativo é implantado para produção, os bancos de dados de desenvolvimento e produção estão fora de sincronizado. Este assincronia precisa ser corrigido durante o processo de implantação. Esses desafios serão examinados em tutoriais futuros.

## <a name="finding-a-web-host-provider"></a>Localizar um provedor de Host da Web

Aplicativos ASP.NET podem ser implantados em qualquer servidor web que tem o .NET Framework e os serviços de informações da Internet (IIS) instalados. Você pode hospedar um site do seu computador pessoal, supondo que você tivesse uma conexão de banda larga à Internet e a saber como configurar seu roteador para permitir que as solicitações da web. Você também pode hospedar um site de um computador em uma intranet, como muitas empresas. No entanto, o foco destes tutoriais é hospedar seu site com um provedor de host da web.

> [!NOTE]
> [IIS](https://www.iis.net/) é o servidor de web de classe empresarial da Microsoft. Ele é fornecido com as edições não inicial do Windows, como o Windows Server 2008 e em algumas edições do Windows Vista. Você não precisa instalar o IIS para atender a aplicativos ASP.NET em um ambiente de desenvolvimento, como o Visual Studio inclui o servidor de Web de desenvolvimento do ASP.NET. No entanto, o servidor de Web de desenvolvimento ASP.NET aceita apenas conexões locais e, portanto, não pode ser usado em um ambiente de produção.


Antes de implantar seu site em um provedor de host da web, você deve primeiro decidir quais fazer negócios com a empresa. Há inúmeras web hospedagem empresas no mercado; uma pesquisa por "da empresa de hospedagem na web" retorna mais de cinco milhões de resultados. Como localizar aquele que é ideal para você? O mecanismo de pesquisa favorito é um bom ponto de partida, como sites como [TopHosts](http://www.tophosts.com/) e [HostCritique](http://www.hostcritique.net/), qual comparar e contrastar vários serviços de hospedagem. Eu também aviso solicitando que seus colegas e colegas de trabalho para todas as recomendações; Você também pode pedir para recomendações de [hospedagem fórum aberto](https://forums.asp.net/158.aspx) aqui no [Fóruns ASP.NET](https://forums.asp.net/).

Empresas de hospedagem da Web geralmente oferecem planos de hospedagem compartilhados e dedicado planos de hospedagem. Com compartilhado hospeda um host de servidor única web dezenas se não centenas de sites diferentes. Com hospedagem dedicado você concessão de um computador da empresa que atende seu site e o site autônomo. Um plano de hospedagem compartilhado pode incluir suporte para as páginas do ASP.NET, a capacidade de trabalhar com bancos de dados do Microsoft Access, 5 GB de espaço em disco e 100 GB de tráfego mensal de largura de banda para $9,95 por mês. Outro plano de hospedagem compartilhado pode incluir suporte para as páginas do ASP.NET, acesso ao servidor de banco de dados Microsoft SQL Server 2008, 10 GB de espaço em disco e 250 GB de tráfego mensal de largura de banda de US $19,95 por mês. Dedicado planos de hospedagem são geralmente muito mais caros, custos centenas de dólares por mês, mas oferecem um desempenho melhor e mais controle que compartilhado opções de hospedagem. O plano escolhido depende de seu orçamento, a quantidade de tráfego recebe seu site e os recursos que você antecipar que você precisará.

Duas considerações importantes ao escolher um provedor de host da web são qualidade de serviço e atendimento ao cliente. Se você tiver uma pergunta ou um problema de configuração, quanto tempo leva enviar seu problema ao helpdesk do host da web até que você obtenha uma resposta? Os serviços da empresa são confiáveis? Com frequência têm interrupções de banco de dados? Quantas vezes o servidor de email ficar offline? Você sempre pode pedir uma empresa para fornecer detalhes sobre o tempo de atividade e saber mais sobre a política de serviço do cliente, mas uma maneira mais certeiro é solicitar comentários dos clientes atuais e anteriores, o que pode ser feito por meio de email listservs, grupos de notícias e fóruns online .

> [!NOTE]
> Algumas empresas de hospedagem da web se concentrar seus negócios em uma pilha de tecnologia específica, como .NET ou [LÂMPADA](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **um** pache, **M** ySQL, e **P** HP), então certifique-se de que a empresa selecionar hospeda aplicativos ASP.NET. Também verifique para garantir que eles oferecem suporte a versão do ASP.NET que você está usando para criar seu aplicativo. E se você estiver criando um aplicativo orientado a dados, certifique-se de que o host da web oferece o mesmo servidor de banco de dados e a mesma versão que você está usando.


## <a name="summary"></a>Resumo

Aplicativos web ASP.NET normalmente são criados, criados e testados em um ambiente de desenvolvimento local. Quando uma versão estiver pronta para ser liberada, ele é movido para um ambiente de produção. Embora seja possível hospedar sites do ASP.NET em seu computador ou em servidores dentro da sua empresa, muitas empresas e indivíduos escolha terceirizar suas hospedagem em um provedor de host da web.

Esta série de tutoriais examina as etapas para implantar um aplicativo ASP.NET para um provedor de host da web, explorando desafios comuns. Este tutorial oferecida uma visão geral do processo de implantação do ASP.NET e forneceu dicas para localizar um provedor de host da web adequado. O seguinte tutorial examina quais arquivos relacionados ao ASP.NET precisam ser copiados para o ambiente de produção durante a implantação de seu site.

Boa programação!

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Avançar](determining-what-files-need-to-be-deployed-cs.md)
