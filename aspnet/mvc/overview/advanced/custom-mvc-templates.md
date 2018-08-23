---
uid: mvc/overview/advanced/custom-mvc-templates
title: Modelo MVC personalizado | Microsoft Docs
author: joeloff
description: Crie um modelo como uma extensão do VSIX.
ms.author: riande
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 3c14bc6feb144a52773bf7bd4c23df24966a9ebb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831029"
---
<a name="custom-mvc-template"></a>Modelo MVC personalizado
====================
por [Jacques Eloff](https://github.com/joeloff)

A versão de atualização de ferramentas do MVC 3 para Visual Studio 2010 introduziu um Assistente de projeto separado para projetos MVC. A alteração foi conduzida por dois fatores. Em primeiro lugar, a introdução de novos modelos do MVC 3 e suporte a mecanismos de exibição adicionais, como o Razor gerar overcrowding a caixa de diálogo Novo projeto no Visual Studio. Em segundo lugar, os clientes tinham solicitado para pontos de extensibilidade e Assistente do novo projeto MVC seria dispensar a-na oportunidade de responder a essas solicitações.

Adicionar modelos personalizados foi um processo árduo, que dependia usando o registro para tornar os novos modelos visível para o Assistente de projeto do MVC. O autor de um novo modelo tinha que encapsule-a em um MSI para garantir que as entradas de registro necessárias seriam criadas no momento da instalação. A alternativa era fazer um arquivo ZIP que contém o modelo disponível e fazer com que o usuário final crie as entradas de registro manualmente.

Nenhuma das abordagens mencionadas anteriormente é ideal, portanto, decidimos aproveitar a infraestrutura existente fornecida pelo [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) extensões para tornar mais fácil para criar, distribuir e instalar modelos personalizados de MVC, começando com o MVC 4 para o Visual Studio 2012. Estes são alguns dos benefícios fornecidos por essa abordagem:

- Uma extensão do VSIX pode conter vários modelos que dão suporte a idiomas diferentes (c# e Visual Basic) e vários mecanismos de exibição (ASPX e Razor).
- Uma extensão do VSIX pode direcionar várias SKUs do Visual Studio, incluindo SKUs de Express.
- O [Galeria do Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) facilita a distribuir a extensão a um público amplo.
- Extensões VSIX podem ser atualizadas que facilita a criação de correções e atualizações para seus modelos personalizados.

## <a name="prerequisites"></a>Pré-requisitos

- Os usuários precisam estar familiarizados com a criação de modelos de projeto, incluindo a marcação necessária para os arquivos de vstemplate etc.
- Os usuários precisarão ter o Visual Studio Professional e superior instalado. SKUs de Express não dão suporte a criar projetos VSIX.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) instalado.

## <a name="example"></a>Exemplo

A primeira etapa é criar um novo projeto VSIX usando c# ou Visual Basic. Selecione **arquivo > Novo projeto**, em seguida, clique em **extensibilidade** no painel esquerdo e selecione o **projeto VSIX**.

![Novo Projeto](custom-mvc-templates/_static/image1.jpg)

Depois que o projeto é criado, o designer VSIX será aberto.

![Metadados de Designer de projeto](custom-mvc-templates/_static/image2.jpg)

O designer pode ser usado para editar algumas das propriedades gerais da extensão que será exibido aos usuários quando instalar a extensão ou procurar as extensões instaladas no Visual Studio (**Ferramentas > extensões e atualizações**). Depois de concluir as informações gerais de clique no **guia destinos de instalação**.

![Destinos de instalação do Designer de projeto](custom-mvc-templates/_static/image3.jpg)

Este guia é usada para especificar os SKUs e versões do Visual Studio que são compatíveis com sua extensão. Selecione a caixa de seleção **este VSIX está instalado para todos os usuários** para permitir instalações de por máquina do VSIX. Clique no **New** botão à direita para adicionar SKUs adicionais, como VWD Web Developer Express ().

![Adicionar novo destino de instalação](custom-mvc-templates/_static/image4.jpg)

Se você pretende dar suporte a todos os profissionais e superior SKUs (Professional, Premium e Ultimate) precisará selecionar o SKU mínimo da família **Microsoft.VisualStudio.Pro**. Lembre-se de salvar todas as alterações depois de ter concluído os destinos de instalação.

![Destinos de instalação do Designer de projeto](custom-mvc-templates/_static/image5.jpg)

O **ativos** guia é usada para adicionar todos os arquivos de conteúdo para o VSIX. Uma vez que o MVC requer metadados personalizados, você alterará o XML bruto do arquivo de manifesto VSIX em vez de usar o **ativos** tab para adicionar conteúdo. Comece adicionando o conteúdo do modelo para o projeto do VSIX. É importante que a estrutura da pasta e o conteúdo espelha o layout do projeto. O exemplo a seguir contém quatro modelos de projeto que foram obtidos com o modelo de projeto do MVC básico. Certifique-se de que todos os arquivos que compõem seu modelo de projeto (tudo sob a pasta ProjectTemplates) são adicionados para o **conteúdo** itemgroup no VSIX do projeto de arquivos e que cada item contém o  **CopyToOutputDirectory** e **IncludeInVsix** metadados definidos conforme mostrado no exemplo a seguir.

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

Caso contrário, o IDE irá tentar compilar o conteúdo do modelo quando você compila o VSIX e você provavelmente verá um erro. Arquivos de código em modelos geralmente contêm especial [parâmetros de modelo](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) usado pelo Visual Studio quando o modelo de projeto é instanciado e, portanto, não pode ser compilado no IDE.

![Gerenciador de Soluções](custom-mvc-templates/_static/image6.jpg)

Feche o designer VSIX, clique com o botão direito no **source.extension.manifest** do arquivo no **Gerenciador de soluções** e selecione **abrir com** e escolha o **(XML Editor de texto)** opção.

![Abra a caixa de diálogo](custom-mvc-templates/_static/image7.jpg)

Criar uma **&lt;ativos&gt;** elemento e adicione um **&lt;ativo&gt;** elemento para cada arquivo que deve ser incluído no VSIX. O **tipo** atributo de cada **&lt;ativo&gt;** elemento deve ser definido como **Microsoft.VisualStudio.Mvc.Template**. Isso é um namespace personalizado que compreende o Assistente de projeto MVC. Consulte a documentação do esquema do VSIX 2.0 para obter mais informações sobre a estrutura e o layout do arquivo de manifesto.

Apenas adicionar os arquivos para o VSIX não é suficiente para registrar os modelos com o assistente MVC. Você precisará fornecer informações como o nome do modelo, a descrição, a mecanismos de exibição com suporte e a linguagem de programação para o Assistente do MVC. Essa informação é executada em atributos personalizados associados a **&lt;ativo&gt;** elemento para cada **vstemplate** arquivo.

&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source =&quot;arquivo&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Language=&quot;C#&quot;

ViewEngine=&quot;Aspx&quot;

TemplateId=&quot;MyMvcApplication&quot;

Título =&quot;aplicativo Web básico personalizado&quot;

Descrição =&quot;um modelo personalizado derivado de um aplicativo web do MVC básico (Razor)&quot;

Version=&quot;4.0&quot;/&gt;

Abaixo está uma explicação dos atributos personalizados que devem estar presentes:

- **ProjectType** deve ser definida como MVC.
- **Linguagem** designa a linguagem de desenvolvimento com suporte pelo modelo. Os valores válidos são em C# ou VB.
- **ViewEngine** designa o mecanismo de exibição compatível com o modelo como Aspx ou Razor. Você pode especificar um valor personalizado para esse campo.
- **TemplateId** é usado para agrupar os modelos. Se o valor corresponde a uma ID de modelo existente será substitua modelos previamente registrados com o assistente MVC.
- **Título** designa a descrição abreviada exibida no Assistente do MVC abaixo de cada modelo de projeto.
- **Descrição** designa uma descrição mais detalhada do modelo.

Depois que você adicionou todos os arquivos no manifesto e salvou, você observará que o **ativos** guia no designer de exibirá todos os arquivos, mas não personalizado atributos você adicionou à **&lt;ativo&gt;** elementos para o **vstemplate** arquivos.

![Ativos do Designer de projeto](custom-mvc-templates/_static/image8.jpg)

Tudo o que resta agora é compilar o projeto VSIX e instalá-lo.

Certifique-se de que todas as instâncias do Visual Studio estão fechadas no computador onde você pretende testar a extensão do VSIX. Visual Studio procura novas extensões durante a inicialização, portanto, se o IDE estiver aberto durante a instalação de um VSIX você precisará reiniciar o Visual Studio. No Explorer, clique duas vezes no arquivo VSIX para iniciar o **instalador do VSIX**, clique em **instalar** e, em seguida, inicie o Visual Studio.

![Instalador do VSIX](custom-mvc-templates/_static/image9.jpg)

No menu, selecione **Ferramentas > extensões e atualizações** para confirmar se sua extensão foi instalada. Se o instalador do VSIX relatou erros durante a instalação da extensão, você pode exibir o log do instalador do VSIX para obter mais informações. O log é geralmente criado na **% temp %** pasta do usuário que instalou a extensão, por exemplo **C:\Users\Bob\AppData\Local\Temp**.

![Extensões e atualizações](custom-mvc-templates/_static/image10.jpg)

Depois de fechar a janela, você pode criar um projeto MVC 4 para ver se os novos modelos são mostrados no Assistente do MVC.

![Novo projeto ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Limitações

1. O Assistente do MVC não oferece suporte a modelos personalizados localizados.
2. O assistente não relatará erros se ele não conseguir localizar os modelos personalizados. Se qualquer um dos atributos personalizados necessários estiverem ausentes, o modelo simplesmente precisar ser excluído do assistente.
