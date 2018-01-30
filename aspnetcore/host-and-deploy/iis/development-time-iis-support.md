---
title: Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core
author: shirhatti
description: "Descubra o suporte para depuração de aplicativos do ASP.NET Core quando executado por trás do IIS no Windows Server."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a5f727dd21ac0c6702691df2215c42f4adc0ec27
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core

Por: [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Este artigo descreve o suporte do [Visual Studio](https://www.visualstudio.com/vs/) a depuração de aplicativos do ASP.NET Core em execução por trás do IIS no Windows Server. Este tópico explica como habilitar esse recurso e configuração de um projeto.

## <a name="prerequisites"></a>Pré-requisitos

* Visual Studio (2017/versão 15.3 ou posterior)
* Carga de trabalho de desenvolvimento Web e ASP.NET *OU* a carga de trabalho de desenvolvimento multiplataforma do .NET Core

## <a name="enable-iis"></a>Habilitar o IIS

Habilite o IIS. Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela). Marque a caixa de seleção **Serviços de Informações da Internet**.

![Recursos do Windows mostrando a caixa de seleção Serviços de Informações da Internet marcada como um quadrado preto (não uma marca de seleção), indicando que alguns dos recursos do IIS estão habilitados](development-time-iis-support/_static/enable_iis.png)

Se a instalação do IIS requer uma reinicialização, reinicie o sistema.

## <a name="enable-development-time-iis-support"></a>Habilitar suporte ao IIS no tempo de desenvolvimento

Depois que o IIS estiver instalado, inicie o instalador do Visual Studio para modificar a instalação existente do Visual Studio. No instalador, selecione o componente **Suporte ao IIS no tempo de desenvolvimento**. O componente está listado como um componente opcional do painel **Resumo** para a carga de trabalho **Desenvolvimento Web e ASP.NET**. Isso instala o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), que é um módulo nativo do IIS necessário para executar aplicativos do ASP.NET Core.

![Modificando os recursos do Visual Studio: a guia Cargas de Trabalho é selecionada. Na seção Web e Nuvem, o painel ASP.NET e desenvolvimento Web é selecionado. À direita na área do painel de resumo opcional, há uma caixa de seleção para IIS suporte de tempo de desenvolvimento.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configurar o projeto

Crie um novo perfil de inicialização para adicionar suporte ao IIS no tempo de desenvolvimento. No **Gerenciador de Soluções** do Visual Studio, clique com o botão direito do mouse no projeto e selecione **Propriedades**. Selecione a guia **Depurar**. Selecione **IIS** da lista suspensa **Iniciar**. Confirme se o recurso **Iniciar Navegador** está habilitado com a URL correta.

![Janela de propriedades de projeto com a guia Depurar selecionada. As configurações de Perfil e de Inicialização são definidas para o IIS. O recurso de Inicialização do navegador está habilitado com um endereço de http://localhost/WebApplication2. O mesmo endereço também é fornecido no campo URL do Aplicativo da área Configurações do Servidor Web com a opção Habilitar a Autenticação Anônima habilitada.](development-time-iis-support/_static/project_properties.png)

Como alternativa, um perfil de lançamento para adicionar manualmente o [launchSettings.json](http://json.schemastore.org/launchsettings) arquivo no aplicativo:

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

O Visual Studio pode solicitar uma reinicialização se não executar como administrador. Se solicitado, reinicie o Visual Studio.

Parabéns! Neste ponto, o projeto está configurado para suporte de tempo de desenvolvimento do IIS. 

## <a name="additional-resources"></a>Recursos adicionais

* [Hospedar o ASP.NET Core no Windows com o IIS](xref:host-and-deploy/iis/index)
* [Introdução ao Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
