---
uid: whitepapers/side-by-side-with-10
title: Execução do .NET Framework 1.0 e 1.1 do ASP.NET lado a lado | Microsoft Docs
author: rick-anderson
description: Este white paper descreve como instalar o .NET 1.0 e 1.1 do .NET em seu computador, permitindo que um aplicativo Web ASP.NET ser executado em qualquer versão do enquadrar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1b8bcebd59a900e54c759509fd4fc5ad1f4f8576
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391154"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>Execução do ASP.NET lado a lado do .NET Framework 1.0 e 1.1
====================
> Este white paper descreve como instalar o .NET 1.0 e 1.1 do .NET em seu computador, permitindo que um aplicativo Web ASP.NET seja executado em qualquer versão do framework.
> 
> Aplica-se para o ASP.NET 1.0 e ASP.NET 1.1.


No ASP.NET, aplicativos devem estar em execução lado a lado quando eles são instalados no mesmo computador, mas usam diferentes versões do .NET Framework. O tópico a seguir descreve como configurar aplicativos do ASP.NET para execução lado a lado e fornece etapas detalhadas para:

- [Manter o mapeamento de seu aplicativo Web para o .NET Framework versão 1.0 durante a instalação](#1)
- [Mapa de um aplicativo Web para uma versão específica do .NET Framework](#2)
- [Localizar a versão do .NET Framework que está usando um site da Web](#3)

Tradicionalmente, quando um componente ou aplicativo é atualizado em um computador, a versão mais antiga é removida e substituída pela versão mais recente. Se a nova versão não é compatível com a versão anterior, isso geralmente afeta outros aplicativos que usam o componente ou aplicativo. O .NET Framework fornece suporte para execução lado a lado, o que permite que várias versões de um assembly ou aplicativo a ser instalado no mesmo computador ao mesmo tempo. Como várias versões podem ser instaladas simultaneamente, os aplicativos gerenciados podem selecionar qual versão usar sem afetar os aplicativos que usam uma versão diferente.

Por padrão, durante a instalação do .NET Framework versão 1.1, todos os aplicativos ASP.NET existentes são automaticamente reconfigurados para usar a versão mais recente do .NET Framework. Se você não quiser seus aplicativos ASP.NET 1.1 do .NET Framework por padrão, clique em [aqui](#1) para saber como evitar isso durante a instalação.

Se você atualizar seu servidor Web para o .NET Framework 1.1 e deseja que um ou mais aplicativos Web para executar o .NET Framework 1.0, você precisará atualizar o mapa de Script do Internet Information Services (IIS). O mapeamento de script é o mecanismo para mapear a extensão de arquivo. aspx para um aplicativo Web específico para uma versão do .NET Framework. Clique em [aqui](#2) para aprender a mapear um aplicativo Web para uma versão específica do .NET Framework.

Você pode usar o Gerenciador de informações da Internet ou a ferramenta de registro ASP.NET IIS (Aspnet\_regiis.exe) para localizar qual versão do .NET Framework está em execução em um aplicativo Web específico. Clique em [aqui](#3) para aprender a localizar a versão do .NET Framework que um site da Web está usando.

Uma consideração de importação ao migrar para o .NET Framework 1.1 é que cada versão do .NET Framework usa seu próprio arquivo Machine. config. Como resultado, se um administrador da Web tiver feito alterações no arquivo Machine. config, essas alterações devem ser migradas para o arquivo Machine. config do .NET Framework 1.1.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Manter o mapeamento de seu aplicativo Web para o .NET Framework 1.0 durante a instalação

Por padrão, todos os aplicativos ASP.NET existentes são automaticamente reconfigurados durante a instalação para usar a versão mais recente do .NET Framework. Usando a versão mais recente do .NET Framework, os aplicativos podem tirar total proveito das melhorias e novos recursos incluídos na nova versão. Ao mesmo tempo, o administrador da Web, que talvez queiram controle granular sobre quais aplicativos são atualizados, pode impedir que o remapeamento automático de todos os aplicativos ASP.NET existentes durante a instalação do .NET Framework.

Para evitar o remapeamento automático de todo o aplicativo ASP.NET para a versão mais recente do .NET Framework, o administrador da Web pode usar a opção de linha de comando /noaspupgrade com o programa de instalação Dotnetfx.exe.

**Para evitar o remapeamento total do aplicativo ASP.NET para a versão mais recente**

1. Vá para **iniciar**.
2. Clique em **executar**.
3. Digite **cmd**.
4. Clique em **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. No prompt de comando, digite a seguinte linha para iniciar a instalação do .NET Framework: **/c Dotnetfx.exe: "instalar /noaspupgrade?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Clique em **Sim** na instalação do Microsoft .NET Framework 1.1. Isso iniciará o processo de instalação do .NET Framework 1.1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Mapa de um aplicativo Web para uma versão específica do .NET Framework

Cada versão do .NET Framework inclui uma versão da ferramenta de registro de IIS do ASP.NET (Aspnet\_regiis.exe). Essa ferramenta permite aos administradores especificar que um aplicativo Web ser executado em uma versão específica do .NET Framework. Isso é chamado de mapeamento de um aplicativo Web para uma versão do .NET Framework. Os administradores devem selecionar o Aspnet\_regiis.exe que corresponde à versão do .NET Framework que será associado com o aplicativo Web. Por exemplo, um administrador que deseja especificar que um site da Web usa o .NET Framework 1.1 deve usar o Aspnet\_regiis.exe que vem com o .NET Framework 1.1.

O Aspnet\_regiis.exe para a versão 1.0 está localizado em:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis

O Aspnet\_regiis.exe para a versão 1,1 está localizado em:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis

O Aspnet\_regiis.exe fornece duas opções para um aplicativo Web de mapeamento de script:

- **-s** define o mapa de script no caminho e no seu filho diretórios.
- **-sn** define o mapa de script no caminho apenas.

O caminho define o caminho da Web aplicativo IIS metadados, que é definido na forma de W3SVC/raiz / {WebSiteNumber} / {aplicativo\_nome}. Por exemplo, para um aplicativo Web chamado Portal localizado no site da Web padrão, o caminho da metabase é W3SVC/1/raiz/Portal.

![](side-by-side-with-10/_static/image4.gif)

Observe que você também pode usar uma ferramenta chamada Editor de Metabase para obter o caminho de metabase. Você pode baixar essa ferramenta no site da Microsoft Support em [ https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Executar Aspnet\_regiis.exe -s W3SVC/1/raiz/Portal para atualizar o portal do IIS de mapa e seu subapplication script.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Executar Aspnet\_regiis.exe -sn W3SVC/1/raiz/Portal para atualizar o script do IIS portal mapear, sem afetar os aplicativos no portal? subdiretórios s.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Localizar a versão do .NET Framework que está usando um aplicativo Web

Um administrador pode usar o Gerenciador de serviços de Internet para encontrar qual versão do .NET Framework é executado em um site da Web. Versões diferentes do sistema operacional inicie o Gerenciador de serviços de Internet de forma diferente. Para iniciar o Gerenciador de serviço, siga as etapas listadas abaixo.

**Para iniciar o Gerenciador de serviço de Internet**

1. Vá para **iniciar**.
2. Clique em **executar**.
3. Tipo de **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. No Gerenciador de serviço de Internet, selecione o aplicativo Web cuja versão do .NET Framework que você deseja saber.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Clique com botão direito no aplicativo Web e, em seguida, clique em **propriedades.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. Na janela Propriedades, selecione **configuração.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. Na tabela de mapeamento do aplicativo, selecione **. aspx**e clique em **editar**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Dos **executável** caixa de texto, examine o diretório de versão ao rolar. Se o diretório de versão é v.1.1.4322, o aplicativo é mapeado para o .NET Framework 1.1. Por outro lado, se o diretório de versão é v1.0.3705, o aplicativo é mapeado para o .NET Framework 1.0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
