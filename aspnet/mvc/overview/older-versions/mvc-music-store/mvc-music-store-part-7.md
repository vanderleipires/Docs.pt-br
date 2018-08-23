---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: Associação e a autorização | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 7 aborda a associação e a autorização.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 44205f0ef59e00ad9fb1c45fdc0ba8934b5804cc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831640"
---
<a name="part-7-membership-and-authorization"></a>Parte 7: Associação e a autorização
====================
por [Jon Galloway](https://github.com/jongalloway)

> A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.  
>   
> A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 7 aborda a associação e a autorização.


Nosso controlador Store Manager é acessível no momento para qualquer pessoa que visitar nosso site. Vamos alterar isso para restringir a permissão para administradores de site.

## <a name="adding-the-accountcontroller-and-views"></a>Adicionando o AccountController e modos de exibição

Uma diferença entre o modelo completo do aplicativo Web do ASP.NET MVC 3 e o modelo de aplicativo de Web vazio do ASP.NET MVC 3 é que o modelo vazio não inclui um controlador de conta. Vamos adicionar um controlador de conta, copiando alguns arquivos de um novo aplicativo ASP.NET MVC criado usando o modelo completo do aplicativo Web do ASP.NET MVC 3.

Criar um novo aplicativo ASP.NET MVC usando o modelo completo do aplicativo Web do ASP.NET MVC 3 e copie os seguintes arquivos para os mesmos diretórios em nosso projeto:

1. AccountController.cs de cópia no diretório controladores
2. AccountModels de cópia no diretório de modelos
3. Crie um diretório de conta dentro do diretório de modos de exibição e copie todos os quatro modos de exibição no

Altere o namespace para as classes de controlador e o modelo para que eles começam com MvcMusicStore. A classe AccountController deve usar o namespace de MvcMusicStore.Controllers e a classe AccountModels deve usar o namespace de MvcMusicStore.Models.

*Observação: Esses arquivos também estão disponíveis no download MvcMusicStore Assets.zip do qual copiamos nossos arquivos de projeto de site no início do tutorial. Os arquivos de associação estão localizados no diretório do código.*

A solução atualizada deve ser semelhante ao seguinte:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Adicionando um usuário administrativo com o site de configuração do ASP.NET

Antes de exigimos autorização em nosso site, precisaremos criar um usuário com acesso. A maneira mais fácil para criar um usuário é usar o site de configuração do ASP.NET interno.

Inicie o site de configuração do ASP.NET, clicando no ícone no Gerenciador de soluções a seguir.

![](mvc-music-store-part-7/_static/image2.png)

Isso inicia um site de configuração. Clique na guia Segurança na tela inicial e, em seguida, clique no link "Ativar funções" no centro da tela.

![](mvc-music-store-part-7/_static/image3.png)

Clique no link "criar ou gerenciar funções".

![](mvc-music-store-part-7/_static/image4.png)

Insira "Administrador" como o nome da função e pressione o botão Adicionar função.

![](mvc-music-store-part-7/_static/image5.png)

Clique no botão Voltar e, em seguida, clique no link Criar usuário no lado esquerdo.

![](mvc-music-store-part-7/_static/image6.png)

Preencha os campos de informações do usuário à esquerda usando as seguintes informações:

| **Campo** | **Value** |
| --- | --- |
| **Nome de usuário** | Administrador |
| **Senha** | password123! |
| **Confirmar senha** | password123! |
| **Email** | (qualquer endereço de email funcionarão) |
| **Pergunta de Segurança** | (o que quiser) |
| **Resposta de Segurança** | (o que quiser) |

*Observação: Você certamente pode usar qualquer senha que você gostaria. As configurações de segurança de senha padrão exigem uma senha que é de 7 caracteres e contém um caractere não alfanumérico.*

Selecione a função de administrador para esse usuário e clique no botão Create User.

![](mvc-music-store-part-7/_static/image7.png)

Neste ponto, você verá uma mensagem indicando que o usuário foi criado com êxito.

![](mvc-music-store-part-7/_static/image8.png)

Agora você pode fechar a janela do navegador.

## <a name="role-based-authorization"></a>Autorização baseada em função

Agora podemos pode restringir o acesso para o StoreManagerController usando o atributo [autorizar], especificando que o usuário deve ser na função de administrador para acessar qualquer ação do controlador na classe.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Observação: O atributo [autorizar] pode ser colocado nos métodos de ação específica, bem como no nível de classe do controlador.*

Agora navegando até /StoreManager abre uma caixa de diálogo de logon:

![](mvc-music-store-part-7/_static/image9.png)

Depois de fazer logon com a nova conta de administrador, podemos ir para a tela Editar álbum como antes.

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-6.md)
> [Próximo](mvc-music-store-part-8.md)
