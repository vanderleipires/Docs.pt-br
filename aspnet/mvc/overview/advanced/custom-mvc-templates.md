---
uid: mvc/overview/advanced/custom-mvc-templates
title: Modelo MVC personalizado | Microsoft Docs
author: joeloff
description: "Crie um modelo como uma extensão do VSIX."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: c3ddd4e341511f520927e924b25d890088adb69e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="custom-mvc-template"></a>Modelo MVC personalizado
====================
por [Jacques Eloff](https://github.com/joeloff)

A versão do MVC 3 ferramentas de atualização para o Visual Studio 2010 introduziu um Assistente de projeto separado para projetos MVC. A alteração foi orientada por dois fatores. Primeiro, a introdução de novos modelos no MVC 3 e suporte para mecanismos de exibição adicionais como Razor gerar overcrowding a caixa de diálogo Novo projeto no Visual Studio. Segundo, os clientes tinham solicitado para pontos de extensibilidade e o Assistente do novo projeto MVC seria nos dispensar a oportunidade de responder a essas solicitações.

Adicionar modelos personalizados: um processo árduo que dependiam usando o registro para que os novos modelos visíveis para o Assistente de projeto MVC. O autor de um novo modelo tinha encapsulá-lo dentro de um MSI para garantir que as entradas de registro necessárias seriam criadas no momento da instalação. A alternativa era fazer um arquivo ZIP que contém o modelo disponível e fazer com que o usuário final criar as entradas de registro manualmente.

Nenhuma das abordagens mencionados acima é ideal para decidimos aproveitar a infraestrutura fornecida por alguns [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) extensões para tornar mais fácil criar, distribuir e instalar modelos personalizados de MVC, começando com MVC 4 para o Visual Studio 2012. Estes são alguns dos benefícios fornecidos por essa abordagem:

- Uma extensão do VSIX pode conter vários modelos que oferecem suporte a idiomas diferentes (c# e Visual Basic) e vários mecanismos de exibição (ASPX e Razor).
- Uma extensão do VSIX pode direcionar várias SKUs do Visual Studio, incluindo Express SKUs.
- O [Galeria do Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) facilita a distribuição de extensão para um público amplo.
- Extensões VSIX podem ser atualizadas que facilita a criação de correções e atualizações para seus modelos personalizados.

## <a name="prerequisites"></a>Pré-requisitos

- Os usuários precisam estar familiarizado com a criação de modelos de projeto, inclusive a marcação necessária para arquivos de vstemplate, etc.
- Os usuários deverão ter o Visual Studio Professional e superior instalado. SKUs Express não dão suporte para criar projetos do VSIX.
- [SDK do Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30668) instalado.

## <a name="example"></a>Exemplo

A primeira etapa é criar um novo projeto do VSIX usando c# ou Visual Basic. Selecione **arquivo > Novo projeto**, em seguida, clique em **extensibilidade** no painel esquerdo e selecione o **projeto VSIX**.

![Novo Projeto](custom-mvc-templates/_static/image1.jpg)

Depois que o projeto é criado, o designer VSIX será aberto.

![Metadados de Designer de projeto](custom-mvc-templates/_static/image2.jpg)

O designer pode ser usado para editar algumas das propriedades gerais da extensão que será mostrado aos usuários quando instalar a extensão ou procurar as extensões instaladas no Visual Studio (**Ferramentas > extensões e atualizações**). Depois de concluir as informações gerais de clique no **guia destinos instalar**.

![Destinos de instalação do Designer de projeto](custom-mvc-templates/_static/image3.jpg)

Esta guia é usada para especificar os SKUs e versões do Visual Studio que são suportadas pela extensão. Selecione a caixa de seleção **este VSIX é instalado para todos os usuários** para habilitar a instalação por máquina do VSIX. Clique no **novo** botão à direita para adicionar os SKUs adicionais como VWD Web Developer Express ().

![Adicionar novo destino de instalação](custom-mvc-templates/_static/image4.jpg)

Se você pretende dar suporte a todos os profissionais e superior SKUs (Professional, Premium e Ultimate) você precisa selecionar apenas a SKU mínima da família, **Microsoft.VisualStudio.Pro**. Lembre-se de salvar todas as alterações depois de concluir os destinos de instalação.

![Destinos de instalação do Designer de projeto](custom-mvc-templates/_static/image5.jpg)

O **ativos** guia é usada para adicionar todos os arquivos de conteúdo para o VSIX. Como o MVC requer metadados personalizados, você alterará o XML bruto do arquivo de manifesto do VSIX em vez de usar o **ativos** guia para adicionar conteúdo. Comece adicionando o conteúdo do modelo para o projeto do VSIX. É importante que a estrutura da pasta e o conteúdo espelha o layout do projeto. O exemplo a seguir contém quatro modelos de projeto que foram derivados do modelo de projeto MVC básico. Certifique-se de que todos os arquivos que compõem o modelo de projeto (tudo sob a pasta ProjectTemplates) são adicionados para o **conteúdo** itemgroup no VSIX arquivo e que contém cada item de projeto de  **CopyToOutputDirectory** e **IncludeInVsix** metadados definidas conforme mostrado no exemplo a seguir.

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

Caso contrário, o IDE tentará compilar o conteúdo do modelo quando você compila o VSIX e você provavelmente verá um erro. Arquivos de código em modelos geralmente contêm especial [parâmetros de modelo](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) usado pelo Visual Studio quando o modelo de projeto é instanciado e, portanto, não pode ser compilado no IDE.

![Gerenciador de Soluções](custom-mvc-templates/_static/image6.jpg)

Fechar o designer VSIX, em seguida, clique com o botão direito no **source.extension.manifest** arquivo **Gerenciador de soluções** e selecione **abrir com** e escolha o **(XML Editor de texto)** opção.

![Abra a caixa de diálogo](custom-mvc-templates/_static/image7.jpg)

Criar um  **&lt;ativos&gt;**  elemento e adicione um  **&lt;ativos&gt;**  elemento para cada arquivo que deve ser incluído no VSIX. O **tipo** atributo de cada  **&lt;ativos&gt;**  elemento deve ser definido como **Microsoft.VisualStudio.Mvc.Template**. Isso é um namespace personalizado que reconheça o Assistente de projeto MVC. Consulte a documentação do esquema do VSIX 2.0 para obter informações adicionais sobre a estrutura e o layout do arquivo de manifesto.

Não é suficiente para registrar os modelos com o assistente MVC apenas adicionar arquivos ao VSIX. Você precisa fornecer informações como o nome do modelo, a descrição, a mecanismos de exibição com suporte e a linguagem de programação para o assistente MVC. Essa informação é executada em atributos personalizados associados a  **&lt;ativos&gt;**  elemento para cada **vstemplate** arquivo.

&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source=&quot;File&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Language=&quot;C#&quot;

ViewEngine=&quot;Aspx&quot;

TemplateId=&quot;MyMvcApplication&quot;

Título =&quot;aplicativo Web básico personalizado&quot;

Descrição =&quot;um modelo personalizado derivado de um aplicativo web do MVC básica (Razor)&quot;

Version=&quot;4.0&quot;/&gt;

Abaixo está uma explicação dos atributos personalizados devem estar presentes:

- **ProjectType** deve ser definido como MVC.
- **Idioma** designa o idioma de desenvolvimento com suporte pelo modelo. Os valores válidos são c# ou VB.
- **ViewEngine** designa o mecanismo de exibição suportado pelo modelo como Aspx ou Razor. Você pode especificar um valor personalizado para esse campo.
- **TemplateId** é usado para agrupar os modelos. Se o valor corresponde a uma ID de modelo existente será substitua modelos previamente registrados com o assistente MVC.
- **Título** designa a descrição curta exibida no Assistente de MVC abaixo de cada modelo de projeto.
- **Descrição** designa uma descrição mais detalhada do modelo.

Depois que você adicionou todos os arquivos para o manifesto e salvo, você observará que o **ativos** guia no designer de exibirá todos os arquivos, mas não o de atributos personalizados adicionados ao  **&lt;ativos&gt;**  elementos para o **vstemplate** arquivos.

![Ativos de Designer de projeto](custom-mvc-templates/_static/image8.jpg)

Tudo o que resta agora é compilar o projeto do VSIX e instalá-lo.

Certifique-se de que todas as instâncias do Visual Studio estão fechadas no computador onde você pretende testar a extensão do VSIX. O Visual Studio procura novas extensões durante a inicialização, portanto, se o IDE estiver aberto durante a instalação de um VSIX será necessário reiniciar o Visual Studio. No Pesquisador de objetos, clique duas vezes no arquivo VSIX para iniciar o **instalador VSIX**, clique em **instalar** e, em seguida, inicie o Visual Studio.

![VSIX Installer](custom-mvc-templates/_static/image9.jpg)

No menu, selecione **Ferramentas > extensões e atualizações** para confirmar se a sua extensão foi instalada. Se o instalador do VSIX relatou erros durante a instalação da extensão, você pode exibir o log do instalador do VSIX para obter mais informações. O log é geralmente criado na **% temp %** pasta do usuário que instalou a extensão, por exemplo **C:\Users\Bob\AppData\Local\Temp**.

![Extensões e atualizações](custom-mvc-templates/_static/image10.jpg)

Depois de fechar a janela, você pode criar um projeto MVC 4 para ver se os novos modelos são mostrados no Assistente de MVC.

![Novo projeto ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Limitações

1. O assistente MVC não dá suporte a modelos personalizados localizados.
2. O assistente não relatará erros caso não consiga localizar modelos personalizados. Se qualquer um dos atributos personalizados necessários estão ausentes, o modelo seria simplesmente excluído do assistente.
