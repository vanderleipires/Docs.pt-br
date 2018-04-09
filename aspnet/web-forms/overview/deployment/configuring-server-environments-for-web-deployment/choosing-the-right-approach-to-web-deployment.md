---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Escolhendo a abordagem certa para a implantação da Web | Microsoft Docs
author: jrjlee
description: Quando você trabalha com o Internet Information Services (IIS) da ferramenta de implantação (implantação da Web) 2.0 ou posterior, há três abordagens principais que você pode usar para obter...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2d690744687af93a69743dc6ce6c853629f61f5d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Escolhendo a abordagem certa para a implantação da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Quando você trabalha com o Internet Information Services (IIS) da ferramenta de implantação (implantação da Web) 2.0 ou posterior, há três abordagens principais que você pode usar para obter seus aplicativos web empacotados em um servidor web. Você pode:
> 
> - Implantar o aplicativo de um local remoto direcionando o *o serviço de agente de implantação Web* (também conhecido como o "agente remoto") no servidor de destino.
> - Implante o aplicativo de um local remoto usando implantar sob demanda da Web (também conhecido como "temp agente").
> - Implantar o aplicativo de um local remoto direcionando o *manipulador de implantação da Web de IIS* no servidor de destino.
> - Implante o aplicativo manualmente copiando o pacote da web para o servidor de destino e importá-lo por meio do Gerenciador do IIS.
> 
> Como configurar os servidores de web de destino dependem de qual abordagem de implantação que você deseja usar. Este tópico o ajudará a decidir qual abordagem de implantação é adequada para você.


Esta tabela mostra as principais vantagens e desvantagens de cada abordagem de implantação, juntamente com os cenários que geralmente se adequar cada abordagem.

| Abordagem | Vantagens | Desvantagens | Cenários típicos |
| --- | --- | --- | --- |
| Agente remoto | É fácil de configurar. Ele é adequado para atualizações regulares em aplicativos da web e o conteúdo. | O usuário deve ser um administrador no servidor de destino. O usuário não pode fornecer credenciais alternativas. | Ambientes de desenvolvimento. Ambientes de teste. |
| Agente Temp | Não é necessário para instalar a implantação da Web no computador de destino. A versão mais recente da implantação da Web é usada automaticamente. | O usuário deve ser um administrador no servidor de destino. O usuário não pode fornecer credenciais alternativas. | Ambientes de desenvolvimento. Ambientes de teste. |
| Manipulador de implantação da Web | Os usuários não administradores podem implantar o conteúdo. Ele é adequado para atualizações regulares em aplicativos da web e o conteúdo. | É muito mais complexa para configurar. | Ambientes de preparo. Ambientes de produção de intranet. Ambientes hospedados. |
| Implantação offline | É muito fácil de configurar. Ele é adequado para ambientes isolados. | O administrador do servidor manualmente deve copiar e importar o pacote da web toda vez. | Ambientes de produção para a Internet. Ambientes de rede isolado. |
  

## <a name="using-the-remote-agent"></a>Usando o agente remoto

Quando você instala a implantação da Web usando as configurações padrão em um servidor de destino, o serviço de agente de implantação da Web (o "agente remoto") é automaticamente instalado e iniciado. Por padrão, o agente remoto expõe um ponto de extremidade HTTP no endereço:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> Você pode substituir [*server*] com o nome da máquina do servidor web, um endereço IP para seu servidor web ou um nome de host resolvida em seu servidor web.


Os administradores de servidor podem implantar pacotes de web de um local remoto, como um computador de um desenvolvedor ou um servidor de compilação, especificando esse endereço de ponto de extremidade. Por exemplo, suponha que Matt Hink Fabrikam, Inc. criou o projeto de aplicativo web ContactManager.Mvc em sua máquina de desenvolvedor. O processo de compilação gera um pacote da web, junto com um *. Deploy* arquivo que contém os comandos de implantação da Web necessárias para instalar o pacote. Se Matt for um administrador de servidor no servidor TESTWEB1, ele pode implantar o aplicativo web para o servidor web de teste executando este comando em sua máquina de desenvolvedor:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


Na verdade real, o executável de implantação da Web pode inferir o endereço do ponto de extremidade do agente remoto se você fornecer o nome da máquina, assim, Matt só precisa digitar:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Para obter mais informações sobre a sintaxe de linha de comando de implantação da Web e *. Deploy* arquivos, consulte [como: instalar uma implantação de pacote usando o arquivo Deploy](https://msdn.microsoft.com/library/ff356104.aspx).


O agente remoto oferece uma maneira simples de implantar o conteúdo de um local remoto, e essa abordagem pode funcionar bem com um clique ou automatizada de implantação. No entanto, o usuário que executa o comando de implantação também deve ser um administrador de domínio ou um membro do grupo Administradores local no servidor de destino. Além disso, o agente remoto não dá suporte a autenticação básica, de forma que você não pode passar credenciais alternativas na linha de comando.

O agente remoto fornece uma abordagem útil para implantação em desenvolvimento ou cenários de teste, onde não é incomum para os desenvolvedores têm controle de administrador completo em um ambiente de servidor de teste e os aplicativos normalmente são recriados e reimplantados muito com frequência. No entanto, essa abordagem é geralmente menor aceitável para ambientes de teste ou produção.

Para obter um exemplo de ponta a ponta de um cenário que usa a abordagem de agente remoto, consulte [cenário: configurar um ambiente de teste para implantação de Web](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Usando o agente Temp

A abordagem de agente temp para implantação é semelhante à abordagem de agente remoto. No entanto, em contraste com a abordagem de agente remoto, você não precisa instalar a implantação da Web no servidor web de destino. Em vez disso, quando você executar a implantação, implantação da Web instalará uma versão temporária do serviço de agente de implantação da web no servidor de destino e usará para implantar seu conteúdo para o IIS. Quando a implantação for concluída, todos os arquivos temporários são removidos.

Se você quiser usar a configuração do provedor de agente temp, adicione o **/g** sinalizador ao seu comando de implantação:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> Você não pode usar o agente temporário se o serviço de agente de implantação da web estiver instalado no computador de destino, mesmo se o serviço não está em execução.


A vantagem dessa abordagem é que você não precisa manter instalações de implantação da Web em seus servidores de destino. Além disso, você não precisa garantir que os computadores de origem e destino estejam executando a mesma versão de implantação da Web. No entanto, essa abordagem tem as mesmas limitações de entidade de segurança que a abordagem de agente remoto, ou seja, que deve ser um administrador local no servidor de destino para implantar conteúdo, e tem suporte somente a autenticação NTLM. A abordagem de agente temp também requer a configuração inicial muito mais do ambiente de destino.

Para obter mais informações sobre como usar o agente temp, consulte [como: instalar uma implantação de pacote usando o arquivo Deploy](https://msdn.microsoft.com/library/ff356104.aspx) e [Web implantar sob demanda](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Usando a Web implantar manipulador

Para o IIS 7 em diante, a implantação da Web oferece uma abordagem alternativa de implantação por meio do manipulador de implantação da Web IIS. O manipulador de implantação da Web está intimamente integrado ao serviço de gerenciamento do IIS da Web (WMSvc), que é projetado para permitir que os usuários gerenciem os sites do IIS de locais remotos.

Por padrão, o agente remoto expõe um ponto de extremidade HTTP no endereço:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> Você pode substituir [*server*] com o nome da máquina do servidor web, um endereço IP para seu servidor web ou um nome de host resolvida em seu servidor web.


A grande vantagem do manipulador de implantação da Web sobre o agente remoto e o agente temp, é que você pode configurar o IIS para permitir que os usuários não administradores implantar aplicativos e conteúdo em sites específicos do IIS. O manipulador de implantação da Web também oferece suporte a autenticação básica, você pode fornecer credenciais alternativas como parâmetros em seus comandos de implantação da Web. A principal desvantagem é que o manipulador de implantação da Web é inicialmente muito mais complicado instalar e configurar.

No caso de usuários não administradores, o serviço de gerenciamento da Web (WMSvc) só permitirá que o usuário se conectar ao IIS usando uma conexão de nível de site, em vez de uma conexão de nível de servidor. Para acessar um site específico, você pode incluir uma cadeia de caracteres de consulta específica do site no endereço do ponto de extremidade:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Por exemplo, suponha que um processo de compilação é configurado para implantar automaticamente de um aplicativo da web em um ambiente de preparo depois de cada compilação bem-sucedida. Se você usou a abordagem de agente remoto, você precisa fazer a identidade do processo de compilação um administrador em seus servidores de destino. Por outro lado, usando a abordagem de manipulador de implantação da Web você pode dar a um usuário não administrador&#x2014;**FABRIKAM\stagingdeployer** nesse caso&#x2014;permissão para um determinado site IIS e o processo de compilação pode fornecer esses credenciais para implantar o pacote da web.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Para obter mais informações sobre operações de linha de comando de implantação da Web e a sintaxe, consulte [referência de linha de comando de implantação da Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Para obter mais informações sobre como usar o *. Deploy* de arquivos, consulte [como: instalar uma implantação de pacote usando o arquivo Deploy](https://msdn.microsoft.com/library/ff356104.aspx).


O manipulador de implantação da Web fornece uma abordagem útil para implantação em ambientes de produção baseado na intranet, onde o acesso remoto para o servidor está disponível, mas as credenciais de administrador não são, ambientes hospedados e ambientes de preparo.

Para obter um exemplo de ponta a ponta de um cenário que usa o método do manipulador de implantação da Web, consulte [cenário: configurar um ambiente de preparo para a implantação da Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Usando a implantação Offline

Em alguns casos, não é possível ou prático implantar aplicativos e o conteúdo em um site do IIS de um local remoto. Por exemplo, os computadores de origem e de destino podem estar em redes isoladas ou segmentos de rede ou política de firewall pode não permitir acesso remoto.

Em situações como essas, você ainda pode usar o empacotamento e a publicação de recursos de implantação da Web; Você apenas não é possível usá-los de um local remoto. Em vez disso, um administrador no servidor de destino deve copiar o pacote da web no servidor e importá-lo por meio do Gerenciador do IIS.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

A abordagem de implantação offline é normalmente útil em ambientes de produção para a Internet, em que os servidores em uma rede de perímetro podem ter restringido conectividade com computadores na rede interna.

Para obter um exemplo de ponta a ponta de um cenário que usa o método de implantação offline, consulte [cenário: configurar um ambiente de produção para implantação de Web](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre operações de linha de comando de implantação da Web e a sintaxe, consulte [referência de linha de comando de implantação da Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Para obter mais informações sobre como usar o *. Deploy* de arquivos, consulte [como: instalar uma implantação de pacote usando o arquivo Deploy](https://msdn.microsoft.com/library/ff356104.aspx).

Para obter orientação geral sobre as diferentes maneiras em que você pode implantar pacotes de web de um computador remoto, consulte [usando Web implantar remotamente](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Para obter mais informações sobre como usar o Web implantar sob demanda, consulte [Web implantar sob demanda](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-server-environments-for-web-deployment.md)
> [Próximo](scenario-configuring-a-test-environment-for-web-deployment.md)
