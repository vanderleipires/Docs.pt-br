---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
title: ASP.NET (VB) de opções de hospedagem | Microsoft Docs
author: rick-anderson
description: Aplicativos web ASP.NET normalmente são projetados, criados e testado em um ambiente de desenvolvimento local e precisam ser implantados em um ambiente de produção e s...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 492f5ae2-bad7-4107-89a9-f04a9525dee7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
msc.type: authoredcontent
ms.openlocfilehash: 68f12de0b459c01ecf766e09144364a64e2d69d3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817704"
---
<a name="aspnet-hosting-options-vb"></a>Opções de hospedagem do ASP.NET (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_vb.pdf)

> Aplicativos web ASP.NET normalmente são projetados, criados e testados em um ambiente de desenvolvimento local e precisa ser implantado em um ambiente de produção quando ele estiver pronto para lançamento. Este tutorial fornece uma visão geral do processo de implantação e serve como uma introdução para esta série de tutoriais.


## <a name="introduction"></a>Introdução

Aplicativos Web são normalmente projetados, criados e testados em um ambiente de desenvolvimento que é acessível apenas para os programadores trabalhando no site. Depois que o aplicativo está pronto para ser lançado, ele é movido para um ambiente de produção em que o site pode ser acessado por qualquer pessoa na Internet. Esse processo de implantação apresenta uma série de desafios:

- Um ambiente de produção deve existir e ser configurado corretamente, antes que um aplicativo ASP.NET pode ser implantado; Além disso, o ambiente de produção deve ser mantido atualizado com os patches de segurança mais recentes.
- O conjunto correto de arquivos de marcação, arquivos de código e arquivos de suporte deve ser copiado do ambiente de desenvolvimento para o ambiente de produção. Para aplicativos controlados por dados, isso pode exigir copiando o esquema de banco de dados e/ou dados, também.
- Pode haver diferenças de configuração entre os dois ambientes. O servidor banco de dados de email ou de cadeia de caracteres de conexão usado no ambiente de desenvolvimento provavelmente será diferente do ambiente de produção. Além disso, o comportamento do aplicativo pode depender do ambiente. Por exemplo, quando ocorre um erro no desenvolvimento de detalhes do erro podem ser exibidas na tela, mas quando ocorre um erro em produção, uma página de erro amigável deve ser exibida em vez disso, e os detalhes do erro enviado por email para os desenvolvedores.

Para eliminar o primeiro desafio - como configurar e manter um ambiente de produção - muitas pessoas e empresas terceirizem a ambientes de produção *provedores de hospedagem de web*. Um provedor de hospedagem na web é uma empresa que gerencia o ambiente de produção em seu nome. Há inúmeras web provedores de host, cada um com diferentes preços e os níveis de serviço; Consulte a seção "Encontrar um provedor de Host da Web" para obter dicas sobre como localizar esse provedor de serviço.

Este é o primeiro de uma série de tutoriais que examinar as etapas envolvidas na implantação de um aplicativo de web do ASP.NET em um ambiente de produção gerenciado por um provedor de host da web. Ao longo destes tutoriais examinaremos:

- Quais arquivos precisam ser implantados para o provedor de host da web.
- Ferramentas para simplificar o processo de implantação.
- Como implantar um banco de dados.
- Dicas para a implantação de um banco de dados que usa [o provedor de associação baseada em SQL e funções](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), juntamente com maneiras de simular a ferramenta de administração de site em um ambiente de produção.
- Estratégias para atualizar sem problemas o banco de dados em produção com as alterações feitas durante o desenvolvimento.
- Técnicas para o log de erros que ocorrem em maneiras para notificar os desenvolvedores quando ocorre um erro e de produção.

Esses tutoriais são voltados para ser conciso e fornecer instruções passo a passo com capturas de tela suficiente para orientá-lo pelo processo visualmente. Este tutorial inaugural fornece uma visão geral do processo de implantação do ASP.NET e conselhos sobre como localizar um provedor de hospedagem na web. Vamos começar!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Uma visão geral do processo de implantação do ASP.NET

Em resumo, implantando um aplicativo ASP.NET envolve três etapas a seguir:

1. Configure o aplicativo web, o servidor web e o banco de dados no ambiente de produção.
2. Sincronizar as páginas ASP.NET, arquivos de código, os assemblies no `Bin` pasta e arquivos de suporte relacionadas a HTML, como arquivos CSS e JavaScript.
3. Sincronize o esquema de banco de dados e/ou dados.

Normalmente, as informações de configuração para um aplicativo web estão localizadas no `Web.config` de arquivo e inclui cadeias de caracteres de conexão de banco de dados, critérios de tratamento de erros, informações do servidor de email e regras de regravação de URL. Muitas vezes, essa informação é diferente de um aplicativo em desenvolvimento versus o mesmo aplicativo em produção. Por exemplo, ao desenvolver um aplicativo é melhor usar um banco de dados de desenvolvimento para que você está testando não no banco de dados de produção. Como resultado, as cadeias de caracteres de conexão de banco de dados normalmente são diferentes entre os aplicativos de desenvolvimento e produção. Devido a essas diferenças, parte da implantação envolve alterações às informações de configuração do aplicativo da web.

Além das alterações de configuração do aplicativo web, etapa 1 também pode envolver a configuração para o servidor web e o banco de dados. Por exemplo, se uma página ASP.NET cria ou exclui arquivos de um diretório no servidor web, em seguida, o servidor web precisará estar configurado para permitir essas modificações no sistema de arquivos. Da mesma forma, pode haver configurações de permissão ou de autenticação que precisam ser feitas no banco de dados.


Etapa 2 envolve sincronizando o conjunto de páginas do ASP.NET essencial e arquivos de suporte entre os ambientes de desenvolvimento e produção. O conjunto específico de ASP. Arquivos relacionados NET que precisam ser sincronizados entre os dois ambientes depende do tipo de projeto criado no Visual Studio e é a discussão no próximo tutorial  <em>[determinando quais arquivos precisam ser implantados](determining-what-files-need-to-be-deployed-vb.md)</em>. Os tutoriais de terceiro e quarto -  <em>[Implantando seu Site usando FTP](deploying-your-site-using-an-ftp-client-vb.md)</em>e <em>[implantando seu Site usando o Visual Studio](deploying-your-site-using-visual-studio-vb.md)</em> -examinar diferentes ferramentas e técnicas para esses arquivos de sincronização.

Ao criar aplicativos controlados por dados, há geralmente dois bancos de dados que está sendo usados: um para desenvolvimento e outro na produção. Durante o desenvolvimento, o esquema do banco de dados de desenvolvimento pode ser modificada para incluir novas tabelas, colunas, procedimentos armazenados e gatilhos ou pode ser modificada para remover ou renomear objetos de banco de dados existente. Entre a hora em que essas alterações são feitas e a hora em que o aplicativo é implantado em produção, os bancos de dados de desenvolvimento e produção estão fora de sincronizado. Este assincronismo precisa ser corrigido durante o processo de implantação. Esses desafios serão examinados em tutoriais futuros.

## <a name="finding-a-web-host-provider"></a>Encontrar um provedor de Host da Web

Aplicativos ASP.NET podem ser implantados em qualquer servidor web que tem o .NET Framework e os serviços de informações da Internet (IIS) instalados. Você pode hospedar um site do seu computador pessoal, supondo que você tivesse uma conexão de banda larga com a Internet e a saber como configurar seu roteador para permitir que as solicitações da web. Você também pode hospedar um site de um computador em uma intranet, assim como muitas empresas. No entanto, o foco destes tutoriais é hospedar seu site com um provedor de host da web.

> [!NOTE]
> [IIS](https://www.iis.net/) é o servidor de web de nível empresarial da Microsoft. Ele é fornecido com as edições de não-Home do Windows, como o Windows Server 2008 e em algumas edições do Windows Vista. Você não precisa instalar o IIS para servir aplicativos ASP.NET em um ambiente de desenvolvimento, como o Visual Studio inclui o servidor de Web de desenvolvimento do ASP.NET. No entanto, o servidor Web de desenvolvimento ASP.NET aceita apenas conexões locais e, portanto, não pode ser usado em um ambiente de produção.


Antes de implantar seu site em um provedor de host da web, primeiro você deve decidir o que fazer negócios com a empresa. Há inúmeras web empresas no marketplace; de hospedagem uma pesquisa por "da empresa de hospedagem na web" retorna mais de cinco milhões de resultados. Como encontrar aquele que é ideal para você? Seu mecanismo de pesquisa favorito é um bom ponto de partida, como sites, como [TopHosts](http://www.tophosts.com/) e [HostCritique](http://www.hostcritique.net/), que comparam e contrastam vários serviços de hospedagem. Eu também de aviso solicitando que seus colegas e colegas de trabalho para todas as recomendações; Você também pode solicitar recomendações na [hospedagem Open Forum](https://forums.asp.net/158.aspx) aqui na [fóruns do ASP.NET](https://forums.asp.net/).

Empresas de hospedagem da Web normalmente oferecem planos de hospedagem compartilhados e dedicado a planos de hospedagem. Com hospedagem compartilhada um hosts de servidor web único dezenas se não centenas de diferentes sites. Com a hospedagem dedicada, você concede um computador da empresa, que serve seu site e seu site sozinho. Um plano de hospedagem compartilhado pode incluir suporte para páginas ASP.NET, a capacidade de trabalhar com bancos de dados do Microsoft Access, 5 GB de espaço em disco e de tráfego mensal de largura de banda de 100 GB por US $9,95 por mês. Outro plano de hospedagem compartilhado pode incluir suporte para páginas ASP.NET, acesso ao servidor de banco de dados Microsoft SQL Server 2008, 10 GB de espaço em disco e 250 GB de tráfego mensal de largura de banda por US $19,95 por mês. Dedicado a planos de hospedagem são geralmente muito mais caros, custando centenas de dólares por mês, mas oferecem um desempenho melhor e mais controle do que compartilhado as opções de hospedagem. O plano escolhido depende de seu orçamento, a quantidade de tráfego recebe de seu site e os recursos que você prevê que você precisará.

Duas considerações importantes ao escolher um provedor de host da web são a qualidade de serviço e atendimento ao cliente. Se você tiver uma pergunta ou um problema de configuração, quanto tempo demora envie seu problema ao helpdesk do host da web até que você receberá uma resposta? Serviços da empresa são confiáveis? Com frequência, eles têm interrupções de banco de dados? A frequência com que o servidor de email ficam offline? Você sempre pode pedir uma empresa para fornecer detalhes sobre o tempo de atividade e consultar sua política de serviço do cliente, mas uma maneira mais infalível é solicitar o feedback de clientes atuais e anteriores, o que pode ser feito por meio de fóruns online, grupos de notícias e email listservs .

> [!NOTE]
> Algumas empresas de hospedagem da web se concentrar seus negócios em uma pilha de tecnologia em particular, como .NET ou [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **um** pache, **M** ySQL, e **P** HP), portanto, certifique-se de que a empresa que você selecione hospeda aplicativos ASP.NET. Também verifique se que eles dão suporte a versão do ASP.NET que você está usando para criar seu aplicativo. E se você estiver criando um aplicativo controlado por dados, certifique-se de que o host da web oferece o mesmo servidor de banco de dados e a mesma versão que você está usando.


## <a name="summary"></a>Resumo

Aplicativos web ASP.NET normalmente são projetados, criados e testados em um ambiente de desenvolvimento local. Depois que uma versão estiver pronta para liberação, ele é movido para um ambiente de produção. Embora seja possível para hospedar sites do ASP.NET em seu computador pessoal ou em servidores dentro da sua empresa, muitas empresas e pessoas escolha terceirizar sua hospedagem em um provedor de host da web.

Esta série de tutoriais examina as etapas para implantar um aplicativo ASP.NET para um provedor de host da web, explorando os desafios comuns. Este tutorial oferecida uma visão geral do processo de implantação do ASP.NET e forneceu dicas para localizar um provedor de host da web adequado. O próximo tutorial examina quais arquivos relacionados ao ASP.NET precisam ser copiados para o ambiente de produção ao implantar seu site.

Boa programação!

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Teresa Murphy. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](users-and-roles-on-the-production-website-cs.md)
> [Próximo](determining-what-files-need-to-be-deployed-vb.md)
