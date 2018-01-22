---
title: "Carregando arquivos em uma página de Razor no ASP.NET Core"
author: guardrex
description: "Saiba como carregar arquivos em uma página do Razor."
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: a29043ac5d28ae8efcbbd6919e73e3774ba22699
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="72cc4-103">Carregando arquivos em uma página de Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72cc4-103">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="72cc4-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="72cc4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="72cc4-105">Nesta seção, o carregamento de arquivos com uma página do Razor é demonstrado.</span><span class="sxs-lookup"><span data-stu-id="72cc4-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="72cc4-106">O [aplicativo de exemplo Filme da páginas do Razor](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) nesse tutorial usa a associação de modelos simples para carregar arquivos, o que funciona bem para carregar arquivos pequenos.</span><span class="sxs-lookup"><span data-stu-id="72cc4-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="72cc4-107">Para obter informações sobre a transmissão de arquivos grandes, consulte [Carregando arquivos grandes com streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="72cc4-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="72cc4-108">Nas etapas a seguir, você adicionará um recurso de upload de arquivo da agenda de filmes ao aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="72cc4-108">In the steps below, you add a movie schedule file upload feature to the sample app.</span></span> <span data-ttu-id="72cc4-109">Um agendamento de filmes é representado por uma classe `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="72cc4-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="72cc4-110">A classe inclui duas versões do agendamento.</span><span class="sxs-lookup"><span data-stu-id="72cc4-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="72cc4-111">Uma versão é fornecida aos clientes, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="72cc4-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="72cc4-112">A outra versão é usada para os funcionários da empresa, `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="72cc4-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="72cc4-113">Cada versão é carregada como um arquivo separado.</span><span class="sxs-lookup"><span data-stu-id="72cc4-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="72cc4-114">O tutorial demonstra como executar dois carregamentos de arquivos de uma página com um único POST para o servidor.</span><span class="sxs-lookup"><span data-stu-id="72cc4-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="72cc4-115">Adicionar uma classe FileUpload</span><span class="sxs-lookup"><span data-stu-id="72cc4-115">Add a FileUpload class</span></span>

<span data-ttu-id="72cc4-116">Abaixo, crie uma página do Razor para lidar com um par de carregamentos de arquivos.</span><span class="sxs-lookup"><span data-stu-id="72cc4-116">Below, you create a Razor page to handle a pair of file uploads.</span></span> <span data-ttu-id="72cc4-117">Adicione uma classe `FileUpload`, que é vinculada à página para obter os dados do agendamento.</span><span class="sxs-lookup"><span data-stu-id="72cc4-117">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="72cc4-118">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="72cc4-118">Right click the *Models* folder.</span></span> <span data-ttu-id="72cc4-119">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="72cc4-119">Select **Add** > **Class**.</span></span> <span data-ttu-id="72cc4-120">Nomeie a classe **FileUpload** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="72cc4-120">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="72cc4-121">A classe tem uma propriedade para o título do agendamento e uma propriedade para cada uma das duas versões do agendamento.</span><span class="sxs-lookup"><span data-stu-id="72cc4-121">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="72cc4-122">Todas as três propriedades são necessárias e o título deve ter 3-60 caracteres.</span><span class="sxs-lookup"><span data-stu-id="72cc4-122">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="72cc4-123">Adicionar um método auxiliar para carregar arquivos</span><span class="sxs-lookup"><span data-stu-id="72cc4-123">Add a helper method to upload files</span></span>

<span data-ttu-id="72cc4-124">Para evitar duplicação de código para processar arquivos do agendamento carregados, primeiro, adicione um método auxiliar estático.</span><span class="sxs-lookup"><span data-stu-id="72cc4-124">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="72cc4-125">Crie uma pasta de *Utilitários* no aplicativo e adicione um arquivo *FileHelpers.cs* com o seguinte conteúdo.</span><span class="sxs-lookup"><span data-stu-id="72cc4-125">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="72cc4-126">O método auxiliar, `ProcessFormFile`, usa um [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) e [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) e retorna uma cadeia de caracteres que contém o tamanho e o conteúdo do arquivo.</span><span class="sxs-lookup"><span data-stu-id="72cc4-126">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="72cc4-127">O comprimento e o tipo de conteúdo são verificados.</span><span class="sxs-lookup"><span data-stu-id="72cc4-127">The content type and length are checked.</span></span> <span data-ttu-id="72cc4-128">Se o arquivo não passar em uma verificação de validação, um erro será adicionado ao `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="72cc4-128">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a><span data-ttu-id="72cc4-129">Adicionar a classe de Agendamento</span><span class="sxs-lookup"><span data-stu-id="72cc4-129">Add the Schedule class</span></span>

<span data-ttu-id="72cc4-130">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="72cc4-130">Right click the *Models* folder.</span></span> <span data-ttu-id="72cc4-131">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="72cc4-131">Select **Add** > **Class**.</span></span> <span data-ttu-id="72cc4-132">Nomeie a classe **Agendamento** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="72cc4-132">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="72cc4-133">Usa a classe usa os atributos `Display` e `DisplayFormat`, que produzem formatação e títulos fáceis quando os dados de agendamento são renderizados.</span><span class="sxs-lookup"><span data-stu-id="72cc4-133">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="72cc4-134">Atualizar o MovieContext</span><span class="sxs-lookup"><span data-stu-id="72cc4-134">Update the MovieContext</span></span>

<span data-ttu-id="72cc4-135">Especifique um `DbSet` no `MovieContext` (*Models/MovieContext.cs*) para os agendamentos:</span><span class="sxs-lookup"><span data-stu-id="72cc4-135">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="72cc4-136">Adicione a tabela de Agendamento ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="72cc4-136">Add the Schedule table to the database</span></span>

<span data-ttu-id="72cc4-137">Abra o Console Gerenciador de pacote (PMC): **Ferramentas** > **Gerenciador de pacote NuGet** > **Console Gerenciador de pacote**.</span><span class="sxs-lookup"><span data-stu-id="72cc4-137">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Menu do PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="72cc4-139">No PMC, execute os seguintes comandos.</span><span class="sxs-lookup"><span data-stu-id="72cc4-139">In the PMC, execute the following commands.</span></span> <span data-ttu-id="72cc4-140">Estes comandos adicionam uma tabela `Schedule` ao banco de dados:</span><span class="sxs-lookup"><span data-stu-id="72cc4-140">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="72cc4-141">Adicionar uma página do Razor de upload de arquivo</span><span class="sxs-lookup"><span data-stu-id="72cc4-141">Add a file upload Razor Page</span></span>

<span data-ttu-id="72cc4-142">Na pasta *Páginas*, crie uma pasta *Agendamentos*.</span><span class="sxs-lookup"><span data-stu-id="72cc4-142">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="72cc4-143">Na pasta *Agendamentos*, crie uma página chamada *Index.cshtml* para carregar um agendamento com o seguinte conteúdo:</span><span class="sxs-lookup"><span data-stu-id="72cc4-143">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="72cc4-144">Cada grupo de formulário inclui um **\<rótulo>** que exibe o nome de cada propriedade de classe.</span><span class="sxs-lookup"><span data-stu-id="72cc4-144">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="72cc4-145">Os atributos `Display` no modelo `FileUpload` fornecem os valores de exibição para os rótulos.</span><span class="sxs-lookup"><span data-stu-id="72cc4-145">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="72cc4-146">Por exemplo, o nome de exibição da propriedade `UploadPublicSchedule` é definido com `[Display(Name="Public Schedule")]` e, portanto, exibe "Agendamento público" no rótulo quando o formulário é renderizado.</span><span class="sxs-lookup"><span data-stu-id="72cc4-146">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="72cc4-147">Cada grupo de formulário inclui uma validação **\<span>**.</span><span class="sxs-lookup"><span data-stu-id="72cc4-147">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="72cc4-148">Se a entrada do usuário não atender aos atributos de propriedade definidos na classe `FileUpload` ou se qualquer uma das verificações de validação do arquivo de método `ProcessFormFile` falhar, o modelo não será validado.</span><span class="sxs-lookup"><span data-stu-id="72cc4-148">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="72cc4-149">Quando a validação do modelo falha, uma mensagem de validação útil é renderizada para o usuário.</span><span class="sxs-lookup"><span data-stu-id="72cc4-149">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="72cc4-150">Por exemplo, a propriedade `Title` é anotada com `[Required]` e `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="72cc4-150">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="72cc4-151">Se o usuário não fornecer um título, ele receberá uma mensagem indicando que um valor é necessário.</span><span class="sxs-lookup"><span data-stu-id="72cc4-151">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="72cc4-152">Se o usuário inserir um valor com menos de três caracteres ou mais de sessenta, ele receberá uma mensagem indicando que o valor tem um comprimento incorreto.</span><span class="sxs-lookup"><span data-stu-id="72cc4-152">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="72cc4-153">Se um arquivo que não tem nenhum conteúdo for fornecido, uma mensagem aparecerá indicando que o arquivo está vazio.</span><span class="sxs-lookup"><span data-stu-id="72cc4-153">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-code-behind-file"></a><span data-ttu-id="72cc4-154">Adicionar o arquivo code-behind</span><span class="sxs-lookup"><span data-stu-id="72cc4-154">Add the code-behind file</span></span>

<span data-ttu-id="72cc4-155">Adicione o arquivo code-behind (*Index.cshtml.cs*) à pasta *Agendamentos*:</span><span class="sxs-lookup"><span data-stu-id="72cc4-155">Add the code-behind file (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="72cc4-156">O modelo de página (`IndexModel` no *Index.cshtml.cs*) associa a classe `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="72cc4-156">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="72cc4-157">O modelo também usa uma lista dos agendamentos (`IList<Schedule>`) para exibir os agendamentos armazenados no banco de dados na página:</span><span class="sxs-lookup"><span data-stu-id="72cc4-157">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="72cc4-158">Quando a página for carregada com `OnGetAsync`, `Schedules` é preenchido com o banco de dados e usado para gerar uma tabela HTML de agendamentos carregados:</span><span class="sxs-lookup"><span data-stu-id="72cc4-158">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="72cc4-159">Quando o formulário é enviado para o servidor, o `ModelState` é verificado.</span><span class="sxs-lookup"><span data-stu-id="72cc4-159">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="72cc4-160">Se for inválido, `Schedule` é recriado e a página é renderizada com uma ou mais mensagens de validação informando por que a validação de página falhou.</span><span class="sxs-lookup"><span data-stu-id="72cc4-160">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="72cc4-161">Se for válido, as propriedades `FileUpload` serão usadas em *OnPostAsync* para concluir o upload do arquivo para as duas versões do agendamento e criar um novo objeto `Schedule` para armazenar os dados.</span><span class="sxs-lookup"><span data-stu-id="72cc4-161">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="72cc4-162">O agendamento, em seguida, é salvo no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="72cc4-162">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="72cc4-163">Vincular a página do Razor de upload de arquivo</span><span class="sxs-lookup"><span data-stu-id="72cc4-163">Link the file upload Razor Page</span></span>

<span data-ttu-id="72cc4-164">Abra *_Layout.cshtml* e adicione um link para a barra de navegação para acessar a página de upload de arquivo:</span><span class="sxs-lookup"><span data-stu-id="72cc4-164">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="72cc4-165">Adicionar uma página para confirmar a exclusão de agendamento</span><span class="sxs-lookup"><span data-stu-id="72cc4-165">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="72cc4-166">Quando o usuário clica para excluir um agendamento, você deseja que ele tenha a oportunidade de cancelar a operação.</span><span class="sxs-lookup"><span data-stu-id="72cc4-166">When the user clicks to delete a schedule, you want them to have a chance to cancel the operation.</span></span> <span data-ttu-id="72cc4-167">Adicione uma página de confirmação de exclusão (*Delete.cshtml*) à pasta *Agendamentos*:</span><span class="sxs-lookup"><span data-stu-id="72cc4-167">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="72cc4-168">O arquivo code-behind (*Delete.cshtml.cs*) carrega um único agendamento identificado por `id` nos dados da rota da solicitação.</span><span class="sxs-lookup"><span data-stu-id="72cc4-168">The code-behind file (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="72cc4-169">Adicione o arquivo *Delete.cshtml.cs* à pasta *Agendamentos*:</span><span class="sxs-lookup"><span data-stu-id="72cc4-169">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="72cc4-170">O método `OnPostAsync` lida com a exclusão do agendamento pelo seu `id`:</span><span class="sxs-lookup"><span data-stu-id="72cc4-170">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="72cc4-171">Após a exclusão com êxito do agendamento, o `RedirectToPage` envia o usuário para a página *Index.cshtml* dos agendamentos.</span><span class="sxs-lookup"><span data-stu-id="72cc4-171">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="72cc4-172">A página de Razor de agendamentos de trabalho</span><span class="sxs-lookup"><span data-stu-id="72cc4-172">The working Schedules Razor Page</span></span>

<span data-ttu-id="72cc4-173">Quando a página é carregada, os rótulos e entradas para o título do agendamento, o agendamento público e o agendamento privado são renderizados com um botão de envio:</span><span class="sxs-lookup"><span data-stu-id="72cc4-173">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Agenda a página de Razor como visto no carregamento inicial sem erros de validação e campos vazios](uploading-files/_static/browser1.png)

<span data-ttu-id="72cc4-175">Selecionar o botão **Upload** sem preencher nenhum dos campos viola os atributos `[Required]` no modelo.</span><span class="sxs-lookup"><span data-stu-id="72cc4-175">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="72cc4-176">O `ModelState` é inválido.</span><span class="sxs-lookup"><span data-stu-id="72cc4-176">The `ModelState` is invalid.</span></span> <span data-ttu-id="72cc4-177">As mensagens de erro de validação são exibidas para o usuário:</span><span class="sxs-lookup"><span data-stu-id="72cc4-177">The validation error messages are displayed to the user:</span></span>

![As mensagens de erro de validação aparecem ao lado de cada controle de entrada](uploading-files/_static/browser2.png)

<span data-ttu-id="72cc4-179">Digite duas letras no campo de **Título**.</span><span class="sxs-lookup"><span data-stu-id="72cc4-179">Type two letters into the **Title** field.</span></span> <span data-ttu-id="72cc4-180">A mensagem de validação muda para indicar que o título deve ter entre 3 e 60 caracteres:</span><span class="sxs-lookup"><span data-stu-id="72cc4-180">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Mensagem de validação de título alterada](uploading-files/_static/browser3.png)

<span data-ttu-id="72cc4-182">Quando um ou mais agendamentos são carregados, a seção **Agendamentos carregados** renderiza os agendamentos carregados:</span><span class="sxs-lookup"><span data-stu-id="72cc4-182">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabela de agendamentos carregados, mostrando o título de cada agendamento, dados carregados em UTC, tamanho do arquivo de versão pública e tamanho do arquivo de versão privada](uploading-files/_static/browser4.png)

<span data-ttu-id="72cc4-184">O usuário pode clicar no link **Excluir** para chegar à exibição de confirmação de exclusão, na qual ele tem a oportunidade de confirmar ou cancelar a operação de exclusão.</span><span class="sxs-lookup"><span data-stu-id="72cc4-184">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="72cc4-185">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="72cc4-185">Troubleshooting</span></span>

<span data-ttu-id="72cc4-186">Para solucionar problemas de informações com o carregamento de `IFormFile`, consulte [Carregamentos de arquivos no ASP.NET Core: solução de problemas](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="72cc4-186">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="72cc4-187">Obrigado por concluir esta introdução às Páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="72cc4-187">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="72cc4-188">Agradecemos todos os comentários deixados.</span><span class="sxs-lookup"><span data-stu-id="72cc4-188">We appreciate any comments you leave.</span></span> <span data-ttu-id="72cc4-189">[Introdução ao MVC e ao EF Core](xref:data/ef-mvc/intro) é um excelente acompanhamento para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="72cc4-189">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="72cc4-190">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="72cc4-190">Additional resources</span></span>

* [<span data-ttu-id="72cc4-191">Carregamentos de arquivos no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72cc4-191">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="72cc4-192">IFormFile</span><span class="sxs-lookup"><span data-stu-id="72cc4-192">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="72cc4-193">Anterior: validação</span><span class="sxs-lookup"><span data-stu-id="72cc4-193">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
