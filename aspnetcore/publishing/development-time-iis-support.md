---
title: Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core
author: shirhatti
description: "Descubra o suporte para depuração de aplicativos do ASP.NET Core quando executado por trás do IIS no Windows Server."
keywords: "ASP.NET Core, Serviços de Informações da Internet, IIS, Windows Server, módulo do ASP.NET Core, depuração"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core

Por: [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Este artigo descreve o suporte do [Visual Studio](https://www.visualstudio.com/vs/) a depuração de aplicativos do ASP.NET Core em execução por trás do IIS no Windows Server. Este tópico orienta você sobre como habilitar esse recurso e configurar seu projeto.

## <a name="prerequisites"></a>Pré-requisitos

* Visual Studio (2017/versão 15.3 ou posterior)
* Carga de trabalho de desenvolvimento Web e ASP.NET *OU* a carga de trabalho de desenvolvimento multiplataforma do .NET Core

## <a name="enable-iis"></a>Habilitar o IIS

Habilite o IIS no seu sistema. Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela). Marque a caixa de seleção **Serviços de Informações da Internet**.

![Recursos do Windows mostrando a caixa de seleção Serviços de Informações da Internet marcada como um quadrado preto (não uma marca de seleção), indicando que alguns dos recursos do IIS estão habilitados](development-time-iis-support/_static/enable_iis.png)

Se a instalação do IIS requer uma reinicialização, reinicie o sistema.

## <a name="enable-development-time-iis-support"></a>Habilitar suporte ao IIS no tempo de desenvolvimento

Depois de instalar o IIS, inicie o instalador do Visual Studio para modificar sua instalação existente do Visual Studio. No instalador, selecione o componente **Suporte ao IIS no tempo de desenvolvimento**. O componente está listado como um componente opcional do painel **Resumo** para a carga de trabalho **Desenvolvimento Web e ASP.NET**. Isso instala o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), que é um módulo nativo do IIS necessário para executar aplicativos do ASP.NET Core.

![Modificando os recursos do Visual Studio: a guia Cargas de Trabalho é selecionada. Na seção Web e Nuvem, o painel ASP.NET e desenvolvimento Web é selecionado. À direita, na área Opcional do painel Resumo, há uma caixa de seleção para suporte ao IIS no tempo de desenvolvimento.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configurar o projeto

Crie um novo perfil de inicialização para adicionar suporte ao IIS no tempo de desenvolvimento. No **Gerenciador de Soluções** do Visual Studio, clique com o botão direito do mouse no projeto e selecione **Propriedades**. Selecione a guia **Depurar**. Selecione **IIS** da lista suspensa **Iniciar**. Confirme se o recurso **Iniciar Navegador** está habilitado com a URL correta.

![Janela de propriedades de projeto com a guia Depurar selecionada. As configurações de Perfil e de Inicialização são definidas para o IIS. O recurso de Inicialização do navegador está habilitado com um endereço de http://localhost/WebApplication2. O mesmo endereço também é fornecido no campo URL do Aplicativo da área Configurações do Servidor Web com a opção Habilitar a Autenticação Anônima habilitada.](development-time-iis-support/_static/project_properties.png)

Como alternativa, você pode adicionar manualmente um perfil de inicialização para o arquivo [launchSettings.json](http://json.schemastore.org/launchsettings) no aplicativo:

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

Pode ser solicitado que você reinicie o Visual Studio se você não estava em executando como administrador. Se solicitado, reinicie o Visual Studio.

Parabéns! Neste ponto, seu projeto está configurado para suporte ao IIS no tempo de desenvolvimento. 

## <a name="additional-resources"></a>Recursos adicionais

* [Hospedar o ASP.NET Core no Windows com o IIS](xref:publishing/iis)
* [Introdução ao Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referência de configuração do Módulo do ASP.NET Core](xref:hosting/aspnet-core-module)
