---
uid: web-forms/what-is-web-forms
title: O que é o Web Forms | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: ccb0e6096b0281e5ef8af63bfd84b172e7f209b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833763"
---
<a name="what-is-web-forms"></a>O que é o Web Forms
====================
Web Forms do ASP.NET é uma parte da estrutura de aplicativo web ASP.NET e está incluído no [Visual Studio](https://www.asp.net/downloads). É um dos quatro modelos de programação que você pode usar para criar aplicativos web ASP.NET, os outros são ASP.NET MVC, páginas da Web ASP.NET e aplicativos de página única do ASP.NET.

Formulários da Web são páginas que os usuários solicitam usando seu navegador. Essas páginas podem ser gravadas usando uma combinação de HTML, script de cliente, os controles de servidor e código do servidor. Quando os usuários solicitam uma página, ele é compilado e executado no servidor pelo framework e, em seguida, a estrutura gera a marcação HTML que pode fazer com que o navegador. Uma página de Web Forms do ASP.NET apresenta informações para o usuário em qualquer navegador ou dispositivo cliente.

Usando o Visual Studio, você pode criar Web Forms do ASP.NET. O Visual Studio Development IDE (ambiente integrado) permite arrastar e soltar os controles de servidor para dispor de sua página de Web Forms. Você pode facilmente definir propriedades, métodos e eventos para controles na página ou para a própria página. Essas propriedades, métodos e eventos são usados para definir o comportamento da página da web, e a aparência e assim por diante. Para escrever código para manipular a lógica para a página do servidor, você pode usar uma linguagem .NET como Visual Basic ou c#.

> [!NOTE] 
> 
> Documentação do ASP.NET e Visual Studio abrange várias versões. Os tópicos que realçam os recursos de versões anteriores podem ser úteis para seus cenários usando as versões mais recentes e as tarefas atuais.


**Web Forms do ASP.NET são:** 

- Com base na tecnologia Microsoft ASP.NET, no qual o código que é executado no servidor dinamicamente gera saída de página da Web para o navegador ou dispositivo cliente.
- É compatível com qualquer navegador ou dispositivo móvel. Uma página da Web do ASP.NET automaticamente renderiza o HTML em conformidade com o navegador correto para recursos, como estilos, layout e assim por diante.
- É compatível com qualquer linguagem compatível com o .NET common language runtime, como Microsoft Visual Basic e o Microsoft Visual c#.
- Construído sobre o Microsoft .NET Framework. Isso fornece todos os benefícios do framework, incluindo um ambiente gerenciado, segurança de tipos e herança.
- Flexível porque você pode adicionar criados pelo usuário e de controles terceirizados para eles.

**Web Forms do ASP.NET oferecem:** 

- Separação de HTML e outro código de interface do usuário da lógica do aplicativo.
- Um conjunto avançado de controles de servidor para tarefas comuns, incluindo acesso a dados.
- Eficientes vinculação de dados, com suporte a excelente ferramenta.
- Suporte para scripts do lado do cliente que é executado no navegador.
- Suporte para uma variedade de outros recursos, incluindo o roteamento, segurança, desempenho, internacionalização, teste, depuração, manipulação de erros e gerenciamento de estado.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>Web Forms do ASP.NET ajuda você a superar os desafios

Programação de aplicativo Web apresenta desafios que normalmente não surgem ao programar aplicativos tradicionais baseados em cliente. Entre os desafios são:

- **Implementando uma interface de usuário da Web avançada** – pode ser difícil e entediante, projetar e implementar um usuário usando os recursos básicos do HTML, especialmente se a página tiver um layout complexo, uma grande quantidade de conteúdo dinâmico, de interface e completos objetos de usuário interativo.
- **Separação entre cliente e servidor** -aplicativo na Web, o cliente (navegador) e o servidor são diferentes de programas em execução em computadores diferentes (e até mesmo em diferentes sistemas operacionais). Consequentemente, as duas metades do aplicativo compartilhem informações muito pouco; eles podem se comunicar, mas normalmente apenas pequenos blocos de informações simples do exchange.
- **Execução sem monitoração de estado** - quando um servidor Web recebe uma solicitação para uma página, ele encontra a página, processa-os, envia para o navegador e, em seguida, descarta todas as informações de página. Se o usuário solicita a mesma página novamente, o servidor repete a sequência inteira, reprocessamento a página a partir do zero. Em outras palavras, um servidor não tem memória de páginas que ele tenha processado — página são sem monitoração de estado. Portanto, se um aplicativo precisar manter informações sobre uma página, sua natureza sem monitoração de estado pode se tornar um problema.
- **Recursos de cliente desconhecido** -em muitos casos, os aplicativos Web são acessíveis a vários usuários que usam diferentes navegadores. Navegadores têm recursos diferentes, tornando difícil criar um aplicativo que será executada igualmente bem em todos eles.
- **Complicações com acesso a dados** -leitura do e gravar em uma fonte de dados em aplicativos Web tradicionais podem ser complicadas e intensivo de recursos.
- **Complicações com escalabilidade** – em muitos casos, os aplicativos Web desenvolvidos com métodos existentes não atender às metas de escalabilidade devido à falta de compatibilidade entre os vários componentes do aplicativo. Isso geralmente é um ponto de falha comum para aplicativos em um ciclo pesado de crescimento.

Enfrentar esses desafios para aplicativos da Web pode exigir esforço e tempo consideráveis. Web Forms do ASP.NET e a estrutura do ASP.NET enfrentam esses desafios das seguintes maneiras:

- **Modelo de objeto consistente e intuitiva** -estrutura de página no ASP.NET apresenta um modelo de objeto que permite que você pensar em seus formulários como uma unidade, e não como partes separadas de cliente e servidor. Nesse modelo, você pode programar a página de uma maneira mais intuitiva do que em aplicativos Web tradicionais, incluindo a capacidade de definir propriedades para elementos de página e responder a eventos. Além disso, os controles de servidor ASP.NET são uma abstração do conteúdo físico de uma página HTML e da interação direta entre navegador e servidor. Em geral, você pode usar controles de servidor a maneira como você pode trabalhar com controles em um aplicativo cliente e não precisará pensar sobre como criar o HTML para apresentar e processar os controles e seus conteúdos.
- **Modelo de programação orientada a eventos** -Web Forms do ASP.NET trazer para aplicativos da Web, o modelo familiar de escrever manipuladores de eventos para eventos que ocorrem no cliente ou servidor. Estrutura de página ASP.NET abstrai esse modelo de tal forma que o mecanismo subjacente de capturar um evento no cliente, transmiti-los para o servidor e chamar o método apropriado é automático e invisível para você. O resultado é uma estrutura clara e facilmente escrita de código que dá suporte ao desenvolvimento controlado por evento.
- **Gerenciamento de estado intuitivo** - estrutura de página no ASP.NET manipula automaticamente a tarefa de manter o estado da sua página e seus controles, e ele fornece maneiras explícitas para manter o estado de informações específicas do aplicativo. Isso é feito sem uso intenso de recursos do servidor e pode ser implementado com ou sem enviar cookies no navegador.
- **Aplicativos independentes de navegador** -estrutura de página no ASP.NET permite que você crie toda a lógica de aplicativo no servidor, eliminando a necessidade de codificação explícita para diferenças em navegadores. No entanto, ele ainda permite tirar proveito dos recursos específicos de navegador escrevendo código do lado do cliente para fornecer melhor desempenho e uma experiência mais avançada do cliente.
- **Suporte do .NET framework common language runtime** -estrutura de página no ASP.NET se baseia no .NET Framework, portanto, a estrutura inteira está disponível para qualquer aplicativo ASP.NET. Os aplicativos podem ser escritos em qualquer linguagem que é compatível com o tempo de execução. Além disso, o acesso a dados é simplificado usando a infraestrutura de acesso de dados fornecida pelo .NET Framework, inclusive o ADO.NET.
- **Desempenho do servidor escalonável do .NET framework** -estrutura de página no ASP.NET permite que você dimensione o seu aplicativo Web de um computador com um único processador para um Web farm de vários computador corretamente e sem alterações complicadas para o aplicativo lógica.

## <a name="features-of-aspnet-web-forms"></a>Recursos do Web Forms do ASP.NET

- **Controles de servidor**-controles de servidor Web do ASP.NET são objetos em páginas da Web ASP.NET que são executados quando a página é solicitada e renderização a marcação para o navegador. Muitos controles de servidor Web são semelhantes a elementos HTML familiares, como botões e caixas de texto. Outros controles possuem um comportamento complexo, como um calendário, controles e controles que você pode usar para se conectar a fontes de dados e exibir dados.
- **Páginas mestras**-páginas mestras do ASP.NET permitem que você crie um layout consistente para as páginas em seu aplicativo. Uma única página mestra define a aparência e o comportamento padrão que você deseja para todas as páginas (ou um grupo de páginas) em seu aplicativo. Em seguida, você pode criar páginas de conteúdo individuais que contêm o conteúdo que você deseja exibir. Quando os usuários solicitam as páginas de conteúdo, eles mesclam com a página mestra para produzir saída que combina o layout da página mestra com o conteúdo da página de conteúdo.
- **Trabalhando com dados**-ASP.NET oferece muitas opções para armazenar, recuperar e exibir dados. Em um aplicativo Web Forms do ASP.NET, você pode usar controles ligados a dados para automatizar a entrada de dados em elementos de interface do usuário da página da web, como tabelas e listas suspensas e caixas de texto ou uma apresentação.
- **Associação**-identidade ASP.NET armazena credenciais dos usuários em um banco de dados criado pelo aplicativo. Quando os usuários fazer logon, o aplicativo valida suas credenciais ao ler o banco de dados. Seu projeto *conta* pasta contém os arquivos que implementam as várias partes da associação: registrar, fazer logon, alterar uma senha e autorizar o acesso. Além disso, o ASP.NET Web Forms dá suporte a OAuth e OpenID. Esses aprimoramentos de autenticação permitem que os usuários façam logon em seu site usando as credenciais existentes, de tais contas como Facebook, Twitter, Windows Live e Google. Por padrão, o modelo cria um banco de dados de associação usando um nome de banco de dados padrão em uma instância do SQL Server Express LocalDB, o servidor de banco de dados de desenvolvimento que vem com o Visual Studio Express 2013 para Web.
- **Script de cliente e estruturas de cliente**-você pode melhorar os recursos baseados em servidor do ASP.NET, incluindo a funcionalidade de script de cliente em páginas de Web Form do ASP.NET. Você pode usar o script de cliente para fornecer uma interface do usuário mais sofisticados e mais ágil na resposta aos usuários. Você também pode usar o script de cliente para fazer chamadas assíncronas para o servidor Web, enquanto uma página está em execução no navegador.
- **Roteamento**-roteamento de URL permite que você configure um aplicativo para aceitar solicitações de URLs que não são mapeadas para arquivos físicos. Uma URL de solicitação é simplesmente a URL em que um usuário digita em seu navegador para encontrar uma página em seu site da web. Usar o roteamento para definir URLs que são semanticamente significativos para os usuários e que podem ajudar com a otimização do mecanismo de pesquisa (SEO).
- **Gerenciamento de estado**-Web Forms do ASP.NET inclui várias opções para ajudar a preservam os dados em uma base por página e uma base de todo o aplicativo.
- **Segurança**-uma parte importante do desenvolvimento de um aplicativo mais seguro é compreender as ameaças a ele. A Microsoft desenvolveu uma maneira de categorizar as ameaças: falsificação, violação, repúdio, divulgação de informações confidenciais, negação de serviço, elevação de privilégios (STRIDE). No Web Forms do ASP.NET, você pode adicionar pontos de extensibilidade e opções de configuração que permitem personalizar vários comportamentos de segurança no Web Forms do ASP.NET.
- **Desempenho**-desempenho pode ser um fator-chave em um projeto ou um site bem-sucedido. Web Forms do ASP.NET permite que você modifique desempenho eficiente de práticas recomendadas de codificação e relacionado à página e processamento de controle de servidor, gerenciamento de estado, acesso a dados, configuração de aplicativo e o carregamento.
- **Internacionalização**- Web Forms do ASP.NET permite que você crie páginas da web que pode obter conteúdo e outros dados com base na configuração de idioma preferencial para o navegador ou com base na opção explícita do usuário da linguagem. Conteúdo e outros dados são conhecidos como recursos e esses dados podem ser armazenadas em arquivos de recurso ou outras fontes. Em uma página de Web Forms do ASP.NET, você deve configurar os controles para obter seus valores de propriedade dos recursos. Em tempo de execução, as expressões de recurso são substituídas por recursos do arquivo de recurso localizado apropriado.
- **Depuração e tratamento de erros**-ASP.NET inclui recursos para ajudá-lo a diagnosticar problemas que podem surgir em seu aplicativo de formulários da Web. Depuração e tratamento de erros também têm suporte no Web Forms do ASP.NET para que seus aplicativos compilar e executar com eficiência.
- **Implantação e hospedagem**-IIS, ASP.NET, Azure e Visual Studio fornecem ferramentas que ajudam com o processo de implantação e hospedagem de seu aplicativo Web Forms.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Decidindo quando criar um aplicativo Web Forms

Você deve considerar cuidadosamente se implementar um aplicativo Web usando o ASP.NET Web Forms modelo ou outro modelo, como o ASP.NET MVC framework. A estrutura do MVC não substitui o modelo de Web Forms; Você pode usar ambas as estruturas para aplicativos da Web. Antes de decidir usar o modelo de Web Forms ou MVC framework para um site específico, pondere as vantagens de cada abordagem.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vantagens de um aplicativo da Web baseada em formulários da Web

A estrutura baseada em Web Forms oferece as seguintes vantagens:

- Ele dá suporte a um modelo de evento que preserva o estado sobre HTTP, que beneficia o desenvolvimento de aplicativos Web de linha de negócios. O aplicativo baseado em Web Forms fornece dezenas de eventos que são suportados em centenas de controles de servidor.
- Ele usa um padrão Page Controller que adiciona funcionalidade para páginas individuais. Para obter mais informações, consulte [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") no site do MSDN.
- Ele usa o estado de exibição nem formulários baseados em servidor, que podem facilitar o gerenciamento de informações de estado.
- Ele funciona bem para pequenas equipes de desenvolvedores da Web e designers que desejam tirar proveito do grande número de componentes disponíveis para desenvolvimento rápido de aplicativos.
- Em geral, é menos complexo para o desenvolvimento de aplicativos, porque os componentes (a **página** classe, controles e assim por diante) estão extremamente integrados e normalmente exigem menos código do que o modelo MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vantagens de um aplicativo Web com base no MVC

O ASP.NET MVC framework oferece as seguintes vantagens:

- Ele torna mais fácil de gerenciar a complexidade ao dividir um aplicativo em modelo, o modo de exibição e o controlador.
- Ele não usa estado de exibição nem formulários baseados em servidor. Isso torna a estrutura MVC ideal para desenvolvedores que desejam controle completo sobre o comportamento de um aplicativo.
- Ele usa um padrão Front Controller que processa as solicitações de aplicativo Web por meio de um único controlador. Isso permite que você criar um aplicativo que dá suporte a uma infraestrutura de roteamento avançada. Para obter mais informações, consulte [controlador frontal](https://go.microsoft.com/fwlink/?LinkId=106357 "controlador frontal") no site do MSDN.
- Ele fornece melhor suporte para o desenvolvimento orientado por testes (TDD).
- Ele funciona bem para aplicativos da Web que são suportados por grandes equipes de desenvolvedores e designers da Web que precisam de um alto grau de controle sobre o comportamento do aplicativo.
