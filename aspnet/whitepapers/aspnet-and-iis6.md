---
uid: whitepapers/aspnet-and-iis6
title: Executando o ASP.NET 1.1 com o IIS 6.0 | Microsoft Docs
author: rick-anderson
description: Embora o Windows Server 2003 inclui o IIS 6.0 e ASP.NET 1.1, esses componentes são desabilitados por padrão. Este white paper descreve como habilitar o IIS 6.0 um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1983ef5b4902acc303ae224d5973eb2644284585
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383419"
---
<a name="running-aspnet-11-with-iis-60"></a>Executando o ASP.NET 1.1 com o IIS 6.0
====================
> Embora o Windows Server 2003 inclui o IIS 6.0 e ASP.NET 1.1, esses componentes são desabilitados por padrão. Este white paper descreve como habilitar o IIS 6.0 e ASP.NET 1.1 e recomenda várias definições de configuração para obter o desempenho ideal do IIS e ASP.NET.
> 
> Aplica-se para o ASP.NET 1.1 e o IIS 6.0.


O ASP.NET 1.1 é fornecido com o Windows Server 2003, que também inclui a versão mais recente do Internet Information Server (IIS) versão 6.0. IIS 6.0 e ASP.NET 1.1 são projetados para integrar perfeitamente e ASP.NET agora assume como padrão para o novo modelo de processo de trabalho do IIS 6.0.

## <a name="aspnet-11-is-not-installed-by-default"></a>O ASP.NET 1.1 não é instalado por padrão

Diferentemente das versões anteriores dos sistemas de operacionais de servidor da Microsoft, o servidor de informações da Internet (IIS) não está habilitado por padrão. nem é ASP.NET 1.1. Há duas opções para permitir que o IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Permitir que o IIS, a opção #1 - o Assistente para configurar o servidor

Windows Server 2003 fornece uma nova 'servidor Assistente para configurar o' para ajudá-lo a configurar corretamente o servidor no modo desejado.

Para iniciar o assistente - Observe que, para executar o assistente, você deve estar conectado como administrador - vá para: Iniciar | Programas | Ferramentas administrativas e selecione 'Configurar o servidor'.

Uma vez selecionada, você deverá ver a tela de abertura 'Configurar o Assistente para o servidor':

![](aspnet-and-iis6/_static/image1.jpg)

Clique em ' Avançar &gt;':

![](aspnet-and-iis6/_static/image2.jpg)

Clique em ' Avançar &gt;'

![](aspnet-and-iis6/_static/image3.jpg)

Nessa tela, será necessário selecionar ' servidor de aplicativos (IIS, ASP.NET) como as opções de configuração.

Clique em ' Avançar &gt;'.

![](aspnet-and-iis6/_static/image4.jpg)

Depois de selecionar esta opção para configurar o servidor como um servidor de aplicativos, essa tela será exibida solicitando que quais recursos adicionais devem ser instalados. Nenhuma opção é selecionada por padrão. Para habilitar o ASP.NET automaticamente, você precisa selecionar ' Habilitar ASP. NET'.

Clique em ' Avançar &gt;'.

![](aspnet-and-iis6/_static/image5.jpg)

Essa tela exibe quais opções estão a serem instalados.

Clique em ' Avançar &gt;'.

![](aspnet-and-iis6/_static/image6.jpg)

Você verá esta tela enquanto as opções selecionadas estão sendo instaladas. É normal para ver outro diálogo caixas aparecem como serviços estão sendo instalados. Além disso, você pode solicitado para o local do CD de instalação do Windows 2003 Server.

Clique em ' Avançar &gt;' quando concluir.

![](aspnet-and-iis6/_static/image7.jpg)

Clique em 'Concluir' - o Windows Server 2003 agora está configurado para dar suporte a IIS 6.0 e ASP.NET 1.1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Habilitar o IIS, a opção #2 - configurar manualmente o IIS e ASP.NET

Se não desejar usar o 'Assistente Configurar o servidor' opcionalmente, você pode instalar o IIS 6.0 e ASP.NET 1.1 usando 'Adicionar ou remover programas' no painel de controle.

Primeiro, abra o painel de controle:

![](aspnet-and-iis6/_static/image8.jpg)

Em seguida, clique em 'Adicionar ou remover Windows Components' que abrirá o Assistente de componentes' Windows':

![](aspnet-and-iis6/_static/image9.jpg)

Realce e verificar o 'servidor de aplicativos ' e, em seguida, clique em 'Detalhes'? Botão:

![](aspnet-and-iis6/_static/image10.jpg)

Para instalar o ASP.NET, verifique ' ASP. NET'.

Clique em 'Okey' para retornar ao Assistente de componentes do Windows. Clique em ' Avançar &gt;' do Assistente de componentes do Windows para começar a instalação:

![](aspnet-and-iis6/_static/image11.jpg)

É normal para ver outro diálogo caixas aparecem como serviços estão sendo instalados. Além disso, você pode solicitado para o local do CD de instalação do Windows 2003 Server.

Quando a instalação for concluída, você verá a última tela do Assistente de componentes do Windows:

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 e ASP.NET 1.1 agora estão configurados e disponíveis.

## <a name="recommended-settings"></a>Configurações recomendadas

Ao executar o ASP.NET 1.1 com o IIS 6.0, existem várias definições de configuração que são recomendadas para obter o desempenho ideal do ASP.NET:

- Configurar limites de memória de processo de trabalho
- Configurando a reciclagem do processo de trabalho

### <a name="configuring-worker-process-memory-limits"></a>Configurar limites de memória de processo de trabalho

Por padrão o IIS 6.0 não definir um limite na quantidade de memória que o IIS tem permissão para usar. ASP. O recurso de Cache da rede se baseia em uma limitação de memória para que o Cache de forma proativa remover itens não usados da memória.

É recomendável que você configure a recurso do IIS 6.0 de reciclagem de memória. Para configurar esse gerenciador de serviços de informações da Internet aberta (Iniciar | Programas | Ferramentas administrativas | Serviços de informações da Internet). Depois de aberto, expanda a pasta de 'Pools de aplicativos ':

Para cada pool de aplicativos:

![](aspnet-and-iis6/_static/image13.jpg)

1. Clique com botão direito no pool de aplicativos, por exemplo 'DefaultAppPool' e selecione 'Propriedades':

![](aspnet-and-iis6/_static/image14.jpg)

2. Em seguida, habilitar a reciclagem de memória clicando em um ' máximo de memória usada (em megabytes):'. O valor não deve ser maior que a quantidade de memória física de (não virtual) no servidor, uma boa aproximação é 60% da memória física, ou seja, para um servidor com 512MB de memória física select 310. Também é recomendável que o máximo não exceder 800MB ao usar um espaço de endereço 2GB. Se o espaço de endereço de memória do servidor é de 3GB, o limite máximo de memória para o processo de trabalho pode ser tão alto como 1, 800MB:

![](aspnet-and-iis6/_static/image15.jpg)

Clique em 'Aplicar' e 'Okey' para sair da caixa de diálogo Propriedades. Repita essa etapa para todos os pools de aplicativos disponíveis.

### <a name="configuring-worker-recycling"></a>Configurando o trabalho de reciclagem

Por padrão, o IIS 6.0 é configurado para reciclar o processo de trabalho a cada 29 horas. Isso é um pouco pesado para um aplicativo executando o ASP.NET e é recomendável que a reciclagem de processos de trabalho automático está desabilitado.

Para desabilitar a reciclagem de processos de trabalho automático, primeiro abra o Gerenciador de serviços de informações da Internet (Iniciar | Programas | Ferramentas administrativas | Serviços de informações da Internet). Depois de aberto, expanda a pasta de 'Pools de aplicativos ':

![](aspnet-and-iis6/_static/image16.jpg)

Para cada pool de aplicativos:

1. Clique com botão direito no pool de aplicativos, por exemplo 'DefaultAppPool' e selecione 'Propriedades':

![](aspnet-and-iis6/_static/image17.jpg)

2. Desmarque a opção ' Reciclar processos do operador (em minutos):':

![](aspnet-and-iis6/_static/image18.jpg)

Clique em 'Aplicar' e 'Okey' para sair da caixa de diálogo Propriedades. Repita essa etapa para todos os pools de aplicativos disponíveis.

## <a name="granting-write-access-to-the-file-system"></a>Conceder acesso de gravação para o sistema de arquivos

Se seu aplicativo requer acesso de gravação para o sistema de arquivos e você estiver usando o NTFS, você precisará modificar uma lista de controle de acesso (ACL) no arquivo ou pasta para conceder acesso ASP.NET a.

Por exemplo, conceder ASP.NET acesso de gravação para o c:\inetpub\wwwroot primeiro abra o explorer e navegue até o diretório:

![](aspnet-and-iis6/_static/image19.jpg)

Em seguida, clique com botão direito no diretório, por exemplo, 'wwwroot' e selecione Propriedades. Depois de abre a caixa de diálogo de propriedades, selecione a guia 'Segurança':

![](aspnet-and-iis6/_static/image20.jpg)

O diretório de c:\inetpub\wwwroot\ é um diretório especial em que o grupo especial de IIS 6.0 ' IIS\_WPG' já foi concedida a leitura &amp; permissões executar, Listar conteúdo da pasta e ler. No entanto, para conceder permissão de gravação, você precisa clicar na caixa de seleção Permitir para gravação:

![](aspnet-and-iis6/_static/image21.jpg)

Agora, o IIS 6.0 tem permissão de gravação nessa pasta. Para conceder permissões de gravação em outras pastas, execute as seguintes etapas – Observe que talvez você precise adicionar o IIS\_grupo WPG se ele ainda não existir.

> [!CAUTION]
> Concedendo a permissão de gravação para o IIS\_WPG permitirá que qualquer aplicativo ASP.NET gravar nesse diretório.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Suporte a autenticação integrada com o SQL Server

Permite a autenticação integrada do SQL Server para aproveitar a autenticação do Windows NT para validar as contas de logon do SQL Server. Isso permite que o usuário ignore o processo de logon padrão do SQL Server. Com essa abordagem, um usuário de rede pode acessar um banco de dados do SQL Server sem fornecer uma identificação de logon separada ou a senha, porque o SQL Server obtém as informações de usuário e senha do processo de segurança de rede do Windows NT.

Escolher a autenticação integrada para aplicativos ASP.NET é uma boa opção porque nenhuma credencial nunca é armazenadas dentro de sua cadeia de caracteres de conexão para seu aplicativo. Em vez disso, a cadeia de conexão usada para se conectar ao SQL será semelhante ao seguinte:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Essa cadeia de caracteres de conexão informa ao SQL Server para usar as credenciais do Windows do aplicativo tentar acessar o SQL Server. No caso de ASP.NET/IIS 6, isso seria uma conta no IIS\_grupo WPG.

Para habilitar a autenticação integrada entre o SQL Server e o ASP.NET, você precisará primeiro certifique-se de que o SQL Server está configurado para a autenticação integrada ou a autenticação de modo misto - entre em contato com seu DBA para determinar isso. Se o SQL Server estiver em um destes dois modos, você pode usar a autenticação integrada.

Abra o SQL Server Enterprise Manager (Iniciar | Programas | Microsoft SQL Server | Enterprise Manager), selecione o servidor apropriado e expanda a pasta de segurança:

![](aspnet-and-iis6/_static/image22.jpg)

Se ' BUILTINT\IIS\_WPG' grupo não estiver listado, clique com botão direito em logons e selecione 'Novo logon':

![](aspnet-and-iis6/_static/image23.jpg)

No ' nome:' caixa de texto Insira ' [nome do domínio do servidor] \IIS\_WPG' ou clique no botão de reticências para abrir o selecionador de usuário/grupo do Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Selecionar o IIS do computador atual\_WPG grupo e clique em 'Adicionar' e Okey para fechar o seletor.

Em seguida, você precisará também definir o banco de dados padrão e as permissões para acessar o banco de dados. Para definir o banco de dados padrão escolha na lista suspensa lista, por exemplo, a Northwind abaixo é selecionado:

![](aspnet-and-iis6/_static/image25.jpg)

Em seguida, clique na guia acesso de banco de dados:

![](aspnet-and-iis6/_static/image26.jpg)

Clique na caixa de seleção para cada banco de dados que você deseja permitir o acesso à permissão. Você também precisará selecionar funções de banco de dados, verificando db\_proprietário garantirá que seu logon tem todas as permissões necessárias para gerenciar e usar o banco de dados selecionado.

Clique em Okey para sair da caixa de diálogo de propriedade. Seu aplicativo ASP.NET agora está configurado para dar suporte à autenticação integrada do SQL Server.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>ASP.NET 1.0 não são executados no modo nativo do IIS 6.0

O ASP.NET 1.0 no IIS 6.0 tem suporte apenas no modo de compatibilidade do IIS 5.

Para configurar o ASP.NET 1.0 para ser executado no modo de compatibilidade do IIS 5.0, abra o Gerenciador de serviços de Internet e Sites da Web de clique com botão direito e selecione Propriedades:

![](aspnet-and-iis6/_static/image27.jpg)

Alterne para a guia de serviço e verificar? Executar serviço WWW no modo de isolamento 5.0 do IIS?:

![](aspnet-and-iis6/_static/image28.jpg)
