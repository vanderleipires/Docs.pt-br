---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: Arquivo -> Novo projeto | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 1 aborda a visão geral e o arquivo/novo projeto.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: d2e4b36c9029e86eea9b09974839e96e9aa39ced
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831046"
---
<a name="part-1-file--new-project"></a>Parte 1: Arquivo -> Novo projeto
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como incrivelmente simples é criar aplicativos avançados e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, incluindo as compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 1 aborda a visão geral e o arquivo/novo projeto.


## <a id="_Toc260221666"></a>  Visão geral

Este tutorial é uma introdução ao ASP.NET WebForms. Podemos vai iniciar lentamente, portanto, a experiência de desenvolvimento para a web de nível iniciante é okey.

O aplicativo, vou desenvolver é um repositório online simples.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Os visitantes podem procurar produtos por categoria:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Eles podem exibir um único produto e adicioná-lo ao seu carrinho:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Eles podem revisar seu carrinho, removendo todos os itens que não deseja mais querem:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Continuar a fazer check-out pedirá para

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Após a ordenação, eles veem uma tela de confirmação simples:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Vamos começar criando um novo projeto de formulários da Web do ASP.NET no Visual Studio 2010, e vamos incrementalmente adicionar recursos para criar um aplicativo funcional completo. Ao longo do caminho, vamos abordar o acesso de banco de dados, modos de exibição de lista e grade, páginas de atualização de dados, validação de dados, usando as páginas mestras para layout de página consistente, AJAX, validação, associação de usuário e muito mais.

Você pode acompanhá-lo passo a passo, ou você pode baixar o aplicativo concluído no [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Você pode usar o Visual Studio 2010 ou o gratuito Visual Web Developer 2010 da [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/). Para compilar o aplicativo, você pode usar o SQL Server ou o SQL Server Express gratuito para hospedar o banco de dados.

## <a id="_Toc260221667"></a>  Arquivo / novo projeto

Vamos começar selecionando o novo projeto no menu arquivo no Visual Studio. Isso abre a caixa de diálogo Novo projeto.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Vamos selecionar Visual C# / modelos da Web de grupo à esquerda e, em seguida, escolha o modelo de "Aplicativo Web do ASP.NET" na coluna central. Nomeie o projeto TailspinSpyworks e pressione o botão Okey.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Isso criará o nosso projeto. Vamos dar uma olhada nas pastas que estão incluídos em nosso aplicativo no Gerenciador de soluções, no lado direito.

![](tailspin-spyworks-part-1/_static/image10.jpg)

Esvaziar a solução não está completamente vazia – ele adiciona uma estrutura de pasta básica:

![](tailspin-spyworks-part-1/_static/image1.png)

Observe as convenções implementadas pelo modelo de projeto padrão ASP.NET 4.

- A pasta "Account" implementa uma interface do usuário básica para ASP. Subsistema de associação da rede.
- A pasta "Scripts" serve como repositório para arquivos de JavaScript do lado do cliente e os arquivos. js do jQuery core estão disponíveis por padrão.
- A pasta "Estilos" é usada para organizar a nossos visuais do site da web (folhas de estilo CSS)

Quando clicamos em F5 para executar nosso aplicativo e renderizar a página Default. aspx, podemos ver o seguinte.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Nossa primeira aprimoramento do aplicativo será substituir o arquivo Style CSS do modelo de formulários da Web padrão com classes CSS e arquivos de imagem associado que processarão o asthetics visual que queremos que nosso aplicativo Tailspin Spyworks.

Depois de fazer isso nossa página Default. aspx renderiza como este.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Observe os links de imagem na parte superior direita da página e os itens de menu que foram adicionados à página mestra. Apenas os links "Entrar" e "Conta" apontam para páginas que existem (geradas pelo modelo padrão) e o restante das páginas que vamos implementar durante a compilação de nosso aplicativo.

Também vamos realocar a página mestra para o diretório de estilos. Embora isso seja apenas uma preferência pode fazer as coisas um pouco mais fácil se podemos decidir fazer nosso aplicativo "skinable" no futuro.

Depois de fazer isso, precisamos alterar a página mestra referências em todos os arquivos. aspx gerada pelo padrão páginas ASP.NET WebForms.

> [!div class="step-by-step"]
> [Avançar](tailspin-spyworks-part-2.md)
