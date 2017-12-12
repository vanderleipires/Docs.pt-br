---
uid: single-page-application/overview/templates/breezeknockout-template
title: "Modelo muito fácil/Knockout | Microsoft Docs"
author: madskristensen
description: "Modelo de aplicativo de página única muito fácil/Knockout"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 07ec099a0381458fe42c1972a2554f76fd34638c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="breezeknockout-template"></a>Modelo muito fácil/Knockout
====================
por [Mads Kristensen](https://github.com/madskristensen)

> O modelo MVC muito fácil/Knockout foi gravado pelo Ward Bell
> 
> [Baixe o modelo MVC muito fácil/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)


Você descobriu do "aplicativo de página única" (SPA) e saber o que é. Enquanto você pode ler sobre ele, em vez disso, experimentaria-lo por conta própria. Mas quem tem tempo para baixar um exemplo? Se você tiver o Visual Studio, você terá um exemplo SPA para cima e em execução em menos de 60 segundos com o ASP.NET MVC 4 modelo "Muito fácil/Knockout aplicativo de página única".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>O que é muito fácil/Knockout SPA modelo?

A maioria dos modelos de projeto geram um esqueleto de aplicativo. Colocar flesh nesses básica, adicionando o código e eventualmente entregar um aplicativo de trabalho. O modelo muito fácil/Knockout SPA é diferente. Ele gera um aplicativo de exemplo para que você estude. Ele demonstra um design de aplicativo do SPA e muitas das técnicas para a criação de um SPA.

O modelo muito fácil/Knockout está uma variação de [modelo KnockoutJS SPA](../introduction/knockoutjs-template.md) incluído no ASP.NET e Web Tools 2012.2 atualização. O modelo muito fácil SPA gera um aplicativo com a mesma experiência de usuário, mas ele tem uma implementação diferente, usando muito fácil para o gerenciamento de dados.

O modelo KnockoutJS SPA faz solicitações de serviço com o jQuery bruto AJAX, que é adequado para um aplicativo simples. Mas aplicativos mais sofisticados têm requisitos de gerenciamento de dados mais exigentes. Por exemplo, a maioria dos aplicativos:

- Consultar e consultar novamente o servidor durante uma sessão de usuário estendido.
- Adicione filtros de consulta, classificação e paginação.
- Compartilhe os mesmos dados em várias telas.
- Acumular alterações a muitos objetos, em seguida, salvá-los como uma única transação.
- Confirmar alterações no cliente, o usuário pode corrigir os erros antes de confirmar as alterações no banco de dados.

A biblioteca de BreezeJS trata essas tarefas para você, de modo que você desenvolver a experiência de usuário e a lógica de aplicativo mais importantes.

[**Muito fácil** ](http://www.breezejs.com/?utm_source=ms-spa) é uma biblioteca de código aberto para criar aplicativos de dados sofisticada em JavaScript e HTML, os tipos de aplicativos que tenham sido historicamente entregues como aplicativos de área de trabalho autônomos.

O modelo muito fácil/Knockout ajuda você a tomar essa primeira etapa essencial para uma infraestrutura de gerenciamento de dados mais robusta. Ele produz um aplicativo de tarefas de exemplo que é idêntico para fora no modelo KnockoutJS SPA. No lado de dentro, ele substitui a camada de dados de AJAX com muito fácil, portanto você pode comparar as duas abordagens lado a lado. Obviamente, quase não toca o potencial de um aplicativo muito fácil. Mas você verá como funciona muito fácil e pouco é necessária para fazer essa transição.

Vamos começar.

## <a name="create-a-breezeknockout-template-project"></a>Criar um projeto de modelo muito fácil/Knockout

Baixe e instale o modelo, clique no botão de Download acima. O modelo é empacotado como um arquivo de extensão de Visual Studio (VSIX). Talvez seja necessário reiniciar o Visual Studio.

No **modelos** painel, selecione **modelos instalados** e expanda o **Visual C#** nó. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto e clique em **Okey**.

No **novo projeto** assistente, selecione **muito fácil Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Pressione Ctrl-F5 para compilar e executar o aplicativo sem depuração, ou pressione F5 para executar com depuração.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

Quando o aplicativo é executado pela primeira vez, ele exibe uma tela de logon. Clique no link "Inscrição" e uma nova página glides no modo de exibição, onde você pode inserir um nome de usuário e senha. (As páginas de logon e registro são criadas usando o ASP.NET MVC). Quando você envia o formulário de registro, o servidor gera uma lista de tarefas com dois itens para sua conta. Em seguida, apresenta-los para você em uma nota amarela.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Agora você está na Terra da controladora. Tudo o que você vê e experiência enquanto manipular Todos é renderizado e gerenciado no cliente com a Ajuda do Knockout e muito fácil. Explore o aplicativo como um usuário... mas com olho do desenvolvedor. Use as ferramentas de desenvolvedor em seu navegador para capturar o tráfego de rede. (No Internet Explorer: pressione F12, selecione o **rede** guia e, em seguida, clique em **iniciar a captura**.) Agora, tente o seguinte:

- Adicione um novo item de tarefas.
- Clique no rótulo e editar o título do item de tarefas
- Marque a caixa de seleção para marcar o item concluído. Observe que a caixa de texto está desabilitada, portanto o título não é mais editável.
- Clique em 'x' à direita do rótulo. O item desaparece e é excluído do banco de dados.
- Escolha outro item e limpar seu título. Você obterá um erro de validação que o título é necessário. Após uma pequena pausa, o título do anterior é restaurado.
- Digite um título ridiculamente longo. Você obterá um erro de validação diferente que o título é muito longo.
- Clique no botão "Adicionar a lista de tarefas". Uma nova lista aparece à esquerda da lista anterior.
- Executar com o título da lista de tarefas, acionando necessário e validações de comprimento.
- Clique na caixa de texto título para limpar a mensagem de erro.
- Clique no círculo no canto superior direito para excluir a lista de tarefas e seu todos "x".

A lógica de validação é executada no lado do cliente por muito fácil. Atributos de validação em classes de modelo do servidor são propagados para o cliente e executados automaticamente antes do cliente contata o servidor.

Analise o tráfego de rede. Observe que não houve nenhuma chamada para o servidor quando muito fácil detectou um erro. Cada alteração válida resultou em uma solicitação POST para "/ tarefas/api/SaveChanges". Muito fácil agrupa as alterações e as envia juntos como uma única solicitação para o controlador de API da Web `SaveChanges` método. Isso é diferente do modelo do SPA KockoutJS, o que torna PUT, POST e excluir solicitações de cada item individualmente.

## <a name="peek-inside"></a>Inspecionar dentro

Este aplicativo tem um cliente e no lado do servidor. A pilha do lado do cliente consiste em HTML um pouco e uma combinação de módulos de JavaScript do aplicativo (na pasta "aplicativo") e bibliotecas JavaScript de terceiros (na pasta "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Se tiver investigado o modelo KnockoutJS SPA, isso deve ser bastante familiar. Concentre-se nas caixas de azul. A arquitetura de interface do usuário é Model-View-ViewModel (MVVM), no qual os widgets HTML do modo de exibição estão corretamente separados do código de apresentação de suporte no modelo de exibição. Um sistema de associação de dados (Knockout neste caso) coordena a exibição e o modelo de exibição para que cada uma pode fazer seu trabalho sem conhecimento profundo do outro.

O modelo encapsula os dados de tarefas. Entidades no modelo são construídas muito fácil com propriedades observáveis do Knockout, para que eles podem ser vinculados diretamente a widgets no modo de exibição. O modelo de exibição solicita o contexto de dados para adquirir e salvar as entidades de modelo. O contexto de dados delega a maior parte do trabalho para muito fácil.

A pilha do lado do servidor consiste em um código de desenvolvedor e três bibliotecas de entidade de segurança do .NET: API da Web, Entity Framework e Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

A arquitetura básica é o mesmo que o modelo KockoutJS SPA. No entanto, a implementação é muito mais simples: O DTOs foram excluídos e a maioria dos detalhes do Entity Framework foram delegada a Breeze.NET.

## <a name="next-steps"></a>Próximas etapas

Sugerimos que você explore o código, seguindo o [discussão abrangente](http://www.breezejs.com/spa-template?utm_source=ms-spa) do cliente e as pilhas de servidor no site muito fácil.

Você pode tentar brincando com consultas de cliente muito fácil; Adicione filtros e classificações. Você pode adicionar mais propriedades do modelo e mais entidades para entender melhor para o desenvolvimento de SPA de ponta a ponta. Quando você tiver certeza do design, você pode desfazer os recursos de Todo e substituí-los com seus próprios.

Assim, você estará pronto para a próxima etapa grande: Adicionar telas do cliente e navegar entre eles. Você vai deixar esse modelo SPA e ative a uma pilha SPA mais completa, como [toalhas ativa do John Papa](https://github.com/johnpapa/HotTowel#readme "toalhas Hot"), que adiciona Durandal à combinação muito fácil e separação.
