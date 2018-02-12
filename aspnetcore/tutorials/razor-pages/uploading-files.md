---
title: "Carregando arquivos em uma página de Razor no ASP.NET Core"
author: guardrex
description: "Saiba como carregar arquivos em uma página do Razor."
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 4a2c6da6ed698d1a65ee51bd00a557e607f012da
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/01/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="0383f-103">Carregando arquivos em uma página de Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0383f-103">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="0383f-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0383f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0383f-105">Nesta seção, o carregamento de arquivos com uma página do Razor é demonstrado.</span><span class="sxs-lookup"><span data-stu-id="0383f-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="0383f-106">O [aplicativo de exemplo Filme da páginas do Razor](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) nesse tutorial usa a associação de modelos simples para carregar arquivos, o que funciona bem para carregar arquivos pequenos.</span><span class="sxs-lookup"><span data-stu-id="0383f-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="0383f-107">Para obter informações sobre a transmissão de arquivos grandes, consulte [Carregando arquivos grandes com streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="0383f-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="0383f-108">Nas etapas a seguir, um recurso de upload de arquivo da agenda de filmes será adicionado ao aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="0383f-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="0383f-109">Um agendamento de filmes é representado por uma classe `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="0383f-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="0383f-110">A classe inclui duas versões do agendamento.</span><span class="sxs-lookup"><span data-stu-id="0383f-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="0383f-111">Uma versão é fornecida aos clientes, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="0383f-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="0383f-112">A outra versão é usada para os funcionários da empresa, `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="0383f-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="0383f-113">Cada versão é carregada como um arquivo separado.</span><span class="sxs-lookup"><span data-stu-id="0383f-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="0383f-114">O tutorial demonstra como executar dois carregamentos de arquivos de uma página com um único POST para o servidor.</span><span class="sxs-lookup"><span data-stu-id="0383f-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="0383f-115">Considerações sobre segurança</span><span class="sxs-lookup"><span data-stu-id="0383f-115">Security considerations</span></span>

<span data-ttu-id="0383f-116">É preciso ter cuidado ao fornecer aos usuários a possibilidade de carregar arquivos em um servidor.</span><span class="sxs-lookup"><span data-stu-id="0383f-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="0383f-117">Os invasores podem executar uma [negação de serviço](/windows-hardware/drivers/ifs/denial-of-service) e outros ataques em um sistema.</span><span class="sxs-lookup"><span data-stu-id="0383f-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="0383f-118">Algumas etapas de segurança que reduzem a probabilidade de um ataque bem-sucedido são:</span><span class="sxs-lookup"><span data-stu-id="0383f-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="0383f-119">Fazer o upload de arquivos para uma área de upload de arquivos dedicada no sistema, o que facilita a aplicação de medidas de segurança sobre o conteúdo carregado.</span><span class="sxs-lookup"><span data-stu-id="0383f-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="0383f-120">Ao permitir os uploads de arquivos, certifique-se de que as permissões de execução estejam desabilitadas no local do upload.</span><span class="sxs-lookup"><span data-stu-id="0383f-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="0383f-121">Use um nome de arquivo seguro determinado pelo aplicativo, não da entrada do usuário ou o nome do arquivo carregado.</span><span class="sxs-lookup"><span data-stu-id="0383f-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="0383f-122">Permitir somente um conjunto específico de extensões de arquivo aprovadas.</span><span class="sxs-lookup"><span data-stu-id="0383f-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="0383f-123">Certifique-se de que as verificações do lado do cliente sejam realizadas no servidor.</span><span class="sxs-lookup"><span data-stu-id="0383f-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="0383f-124">As verificações do lado do cliente são fáceis de contornar.</span><span class="sxs-lookup"><span data-stu-id="0383f-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="0383f-125">Verifique o tamanho do upload e evite uploads maiores do que o esperado.</span><span class="sxs-lookup"><span data-stu-id="0383f-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="0383f-126">Execute um verificador de vírus/malware no conteúdo carregado.</span><span class="sxs-lookup"><span data-stu-id="0383f-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="0383f-127">Carregar códigos mal-intencionados em um sistema é frequentemente a primeira etapa para executar o código que pode:</span><span class="sxs-lookup"><span data-stu-id="0383f-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="0383f-128">Tomar o controle total de um sistema.</span><span class="sxs-lookup"><span data-stu-id="0383f-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="0383f-129">Sobrecarregar um sistema, fazendo com que ele falhe completamente.</span><span class="sxs-lookup"><span data-stu-id="0383f-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="0383f-130">Comprometer dados do sistema ou de usuários.</span><span class="sxs-lookup"><span data-stu-id="0383f-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="0383f-131">Aplicar pichações a uma interface pública.</span><span class="sxs-lookup"><span data-stu-id="0383f-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="0383f-132">Adicionar uma classe FileUpload</span><span class="sxs-lookup"><span data-stu-id="0383f-132">Add a FileUpload class</span></span>

<span data-ttu-id="0383f-133">Criar uma Página Razor para lidar com um par de carregamentos de arquivos.</span><span class="sxs-lookup"><span data-stu-id="0383f-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="0383f-134">Adicione uma classe `FileUpload`, que é vinculada à página para obter os dados do agendamento.</span><span class="sxs-lookup"><span data-stu-id="0383f-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="0383f-135">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="0383f-135">Right click the *Models* folder.</span></span> <span data-ttu-id="0383f-136">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="0383f-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="0383f-137">Nomeie a classe **FileUpload** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="0383f-137">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="0383f-138">A classe tem uma propriedade para o título do agendamento e uma propriedade para cada uma das duas versões do agendamento.</span><span class="sxs-lookup"><span data-stu-id="0383f-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="0383f-139">Todas as três propriedades são necessárias e o título deve ter 3-60 caracteres.</span><span class="sxs-lookup"><span data-stu-id="0383f-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="0383f-140">Adicionar um método auxiliar para carregar arquivos</span><span class="sxs-lookup"><span data-stu-id="0383f-140">Add a helper method to upload files</span></span>

<span data-ttu-id="0383f-141">Para evitar duplicação de código para processar arquivos do agendamento carregados, primeiro, adicione um método auxiliar estático.</span><span class="sxs-lookup"><span data-stu-id="0383f-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="0383f-142">Crie uma pasta de *Utilitários* no aplicativo e adicione um arquivo *FileHelpers.cs* com o seguinte conteúdo.</span><span class="sxs-lookup"><span data-stu-id="0383f-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="0383f-143">O método auxiliar, `ProcessFormFile`, usa um [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) e [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) e retorna uma cadeia de caracteres que contém o tamanho e o conteúdo do arquivo.</span><span class="sxs-lookup"><span data-stu-id="0383f-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="0383f-144">O comprimento e o tipo de conteúdo são verificados.</span><span class="sxs-lookup"><span data-stu-id="0383f-144">The content type and length are checked.</span></span> <span data-ttu-id="0383f-145">Se o arquivo não passar em uma verificação de validação, um erro será adicionado ao `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="0383f-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a><span data-ttu-id="0383f-146">Salvar o arquivo no disco</span><span class="sxs-lookup"><span data-stu-id="0383f-146">Save the file to disk</span></span>

<span data-ttu-id="0383f-147">O aplicativo de exemplo salva o conteúdo do arquivo em um campo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0383f-147">The sample app saves the file's content into a database field.</span></span> <span data-ttu-id="0383f-148">Para salvar o conteúdo do arquivo no disco, use um [FileStream](/dotnet/api/system.io.filestream):</span><span class="sxs-lookup"><span data-stu-id="0383f-148">To save the file's content to disk, use a [FileStream](/dotnet/api/system.io.filestream):</span></span>

```csharp
using (var fileStream = new FileStream(filePath, FileMode.Create))
{
    await formFile.CopyToAsync(fileStream);
}
```

<span data-ttu-id="0383f-149">O processo de trabalho deve ter permissões de gravação para o local especificado por `filePath`.</span><span class="sxs-lookup"><span data-stu-id="0383f-149">The worker process must have write permissions to the location specified by `filePath`.</span></span>

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="0383f-150">Salvar o arquivo no Armazenamento de Blobs do Azure</span><span class="sxs-lookup"><span data-stu-id="0383f-150">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="0383f-151">Para carregar o conteúdo do arquivo para o Armazenamento de Blobs do Azure, confira [Introdução ao Armazenamento de Blobs do Azure usando o .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="0383f-151">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="0383f-152">O tópico demonstra como usar [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) para salvar um [FileStream](/dotnet/api/system.io.filestream) para armazenamento de blobs.</span><span class="sxs-lookup"><span data-stu-id="0383f-152">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="0383f-153">Adicionar a classe de Agendamento</span><span class="sxs-lookup"><span data-stu-id="0383f-153">Add the Schedule class</span></span>

<span data-ttu-id="0383f-154">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="0383f-154">Right click the *Models* folder.</span></span> <span data-ttu-id="0383f-155">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="0383f-155">Select **Add** > **Class**.</span></span> <span data-ttu-id="0383f-156">Nomeie a classe **Agendamento** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="0383f-156">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="0383f-157">Usa a classe usa os atributos `Display` e `DisplayFormat`, que produzem formatação e títulos fáceis quando os dados de agendamento são renderizados.</span><span class="sxs-lookup"><span data-stu-id="0383f-157">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="0383f-158">Atualizar o MovieContext</span><span class="sxs-lookup"><span data-stu-id="0383f-158">Update the MovieContext</span></span>

<span data-ttu-id="0383f-159">Especifique um `DbSet` no `MovieContext` (*Models/MovieContext.cs*) para os agendamentos:</span><span class="sxs-lookup"><span data-stu-id="0383f-159">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="0383f-160">Adicione a tabela de Agendamento ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="0383f-160">Add the Schedule table to the database</span></span>

<span data-ttu-id="0383f-161">Abra o Console Gerenciador de pacote (PMC): **Ferramentas** > **Gerenciador de pacote NuGet** > **Console Gerenciador de pacote**.</span><span class="sxs-lookup"><span data-stu-id="0383f-161">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Menu do PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="0383f-163">No PMC, execute os seguintes comandos.</span><span class="sxs-lookup"><span data-stu-id="0383f-163">In the PMC, execute the following commands.</span></span> <span data-ttu-id="0383f-164">Estes comandos adicionam uma tabela `Schedule` ao banco de dados:</span><span class="sxs-lookup"><span data-stu-id="0383f-164">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="0383f-165">Adicionar uma página do Razor de upload de arquivo</span><span class="sxs-lookup"><span data-stu-id="0383f-165">Add a file upload Razor Page</span></span>

<span data-ttu-id="0383f-166">Na pasta *Páginas*, crie uma pasta *Agendamentos*.</span><span class="sxs-lookup"><span data-stu-id="0383f-166">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="0383f-167">Na pasta *Agendamentos*, crie uma página chamada *Index.cshtml* para carregar um agendamento com o seguinte conteúdo:</span><span class="sxs-lookup"><span data-stu-id="0383f-167">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="0383f-168">Cada grupo de formulário inclui um **\<rótulo>** que exibe o nome de cada propriedade de classe.</span><span class="sxs-lookup"><span data-stu-id="0383f-168">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="0383f-169">Os atributos `Display` no modelo `FileUpload` fornecem os valores de exibição para os rótulos.</span><span class="sxs-lookup"><span data-stu-id="0383f-169">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="0383f-170">Por exemplo, o nome de exibição da propriedade `UploadPublicSchedule` é definido com `[Display(Name="Public Schedule")]` e, portanto, exibe "Agendamento público" no rótulo quando o formulário é renderizado.</span><span class="sxs-lookup"><span data-stu-id="0383f-170">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="0383f-171">Cada grupo de formulário inclui uma validação **\<span>**.</span><span class="sxs-lookup"><span data-stu-id="0383f-171">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="0383f-172">Se a entrada do usuário não atender aos atributos de propriedade definidos na classe `FileUpload` ou se qualquer uma das verificações de validação do arquivo de método `ProcessFormFile` falhar, o modelo não será validado.</span><span class="sxs-lookup"><span data-stu-id="0383f-172">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="0383f-173">Quando a validação do modelo falha, uma mensagem de validação útil é renderizada para o usuário.</span><span class="sxs-lookup"><span data-stu-id="0383f-173">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="0383f-174">Por exemplo, a propriedade `Title` é anotada com `[Required]` e `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="0383f-174">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="0383f-175">Se o usuário não fornecer um título, ele receberá uma mensagem indicando que um valor é necessário.</span><span class="sxs-lookup"><span data-stu-id="0383f-175">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="0383f-176">Se o usuário inserir um valor com menos de três caracteres ou mais de sessenta, ele receberá uma mensagem indicando que o valor tem um comprimento incorreto.</span><span class="sxs-lookup"><span data-stu-id="0383f-176">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="0383f-177">Se um arquivo que não tem nenhum conteúdo for fornecido, uma mensagem aparecerá indicando que o arquivo está vazio.</span><span class="sxs-lookup"><span data-stu-id="0383f-177">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="0383f-178">Adicionar o modelo de página</span><span class="sxs-lookup"><span data-stu-id="0383f-178">Add the page model</span></span>

<span data-ttu-id="0383f-179">Adicione o modelo de página (*Index.cshtml.cs*) à pasta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="0383f-179">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="0383f-180">O modelo de página (`IndexModel` no *Index.cshtml.cs*) associa a classe `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="0383f-180">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="0383f-181">O modelo também usa uma lista dos agendamentos (`IList<Schedule>`) para exibir os agendamentos armazenados no banco de dados na página:</span><span class="sxs-lookup"><span data-stu-id="0383f-181">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="0383f-182">Quando a página for carregada com `OnGetAsync`, `Schedules` é preenchido com o banco de dados e usado para gerar uma tabela HTML de agendamentos carregados:</span><span class="sxs-lookup"><span data-stu-id="0383f-182">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="0383f-183">Quando o formulário é enviado para o servidor, o `ModelState` é verificado.</span><span class="sxs-lookup"><span data-stu-id="0383f-183">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="0383f-184">Se for inválido, `Schedule` é recriado e a página é renderizada com uma ou mais mensagens de validação informando por que a validação de página falhou.</span><span class="sxs-lookup"><span data-stu-id="0383f-184">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="0383f-185">Se for válido, as propriedades `FileUpload` serão usadas em *OnPostAsync* para concluir o upload do arquivo para as duas versões do agendamento e criar um novo objeto `Schedule` para armazenar os dados.</span><span class="sxs-lookup"><span data-stu-id="0383f-185">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="0383f-186">O agendamento, em seguida, é salvo no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="0383f-186">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="0383f-187">Vincular a página do Razor de upload de arquivo</span><span class="sxs-lookup"><span data-stu-id="0383f-187">Link the file upload Razor Page</span></span>

<span data-ttu-id="0383f-188">Abra *_Layout.cshtml* e adicione um link para a barra de navegação para acessar a página de upload de arquivo:</span><span class="sxs-lookup"><span data-stu-id="0383f-188">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="0383f-189">Adicionar uma página para confirmar a exclusão de agendamento</span><span class="sxs-lookup"><span data-stu-id="0383f-189">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="0383f-190">Quando o usuário clica para excluir um agendamento, é oferecida uma oportunidade de cancelar a operação.</span><span class="sxs-lookup"><span data-stu-id="0383f-190">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="0383f-191">Adicione uma página de confirmação de exclusão (*Delete.cshtml*) à pasta *Agendamentos*:</span><span class="sxs-lookup"><span data-stu-id="0383f-191">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="0383f-192">O modelo de página (*Delete.cshtml.cs*) carrega um único agendamento identificado por `id` nos dados de rota da solicitação.</span><span class="sxs-lookup"><span data-stu-id="0383f-192">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="0383f-193">Adicione o arquivo *Delete.cshtml.cs* à pasta *Agendamentos*:</span><span class="sxs-lookup"><span data-stu-id="0383f-193">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="0383f-194">O método `OnPostAsync` lida com a exclusão do agendamento pelo seu `id`:</span><span class="sxs-lookup"><span data-stu-id="0383f-194">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="0383f-195">Após a exclusão com êxito do agendamento, o `RedirectToPage` envia o usuário para a página *Index.cshtml* dos agendamentos.</span><span class="sxs-lookup"><span data-stu-id="0383f-195">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="0383f-196">A página de Razor de agendamentos de trabalho</span><span class="sxs-lookup"><span data-stu-id="0383f-196">The working Schedules Razor Page</span></span>

<span data-ttu-id="0383f-197">Quando a página é carregada, os rótulos e entradas para o título do agendamento, o agendamento público e o agendamento privado são renderizados com um botão de envio:</span><span class="sxs-lookup"><span data-stu-id="0383f-197">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Agenda a página de Razor como visto no carregamento inicial sem erros de validação e campos vazios](uploading-files/_static/browser1.png)

<span data-ttu-id="0383f-199">Selecionar o botão **Upload** sem preencher nenhum dos campos viola os atributos `[Required]` no modelo.</span><span class="sxs-lookup"><span data-stu-id="0383f-199">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="0383f-200">O `ModelState` é inválido.</span><span class="sxs-lookup"><span data-stu-id="0383f-200">The `ModelState` is invalid.</span></span> <span data-ttu-id="0383f-201">As mensagens de erro de validação são exibidas para o usuário:</span><span class="sxs-lookup"><span data-stu-id="0383f-201">The validation error messages are displayed to the user:</span></span>

![As mensagens de erro de validação aparecem ao lado de cada controle de entrada](uploading-files/_static/browser2.png)

<span data-ttu-id="0383f-203">Digite duas letras no campo de **Título**.</span><span class="sxs-lookup"><span data-stu-id="0383f-203">Type two letters into the **Title** field.</span></span> <span data-ttu-id="0383f-204">A mensagem de validação muda para indicar que o título deve ter entre 3 e 60 caracteres:</span><span class="sxs-lookup"><span data-stu-id="0383f-204">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Mensagem de validação de título alterada](uploading-files/_static/browser3.png)

<span data-ttu-id="0383f-206">Quando um ou mais agendamentos são carregados, a seção **Agendamentos carregados** renderiza os agendamentos carregados:</span><span class="sxs-lookup"><span data-stu-id="0383f-206">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabela de agendamentos carregados, mostrando o título de cada agendamento, dados carregados em UTC, tamanho do arquivo de versão pública e tamanho do arquivo de versão privada](uploading-files/_static/browser4.png)

<span data-ttu-id="0383f-208">O usuário pode clicar no link **Excluir** para chegar à exibição de confirmação de exclusão, na qual ele tem a oportunidade de confirmar ou cancelar a operação de exclusão.</span><span class="sxs-lookup"><span data-stu-id="0383f-208">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="0383f-209">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="0383f-209">Troubleshooting</span></span>

<span data-ttu-id="0383f-210">Para solucionar problemas de informações com o carregamento de `IFormFile`, consulte [Carregamentos de arquivos no ASP.NET Core: solução de problemas](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="0383f-210">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="0383f-211">Obrigado por concluir esta introdução às Páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="0383f-211">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="0383f-212">Agradecemos os comentários.</span><span class="sxs-lookup"><span data-stu-id="0383f-212">We appreciate feedback.</span></span> <span data-ttu-id="0383f-213">[Introdução ao MVC e ao EF Core](xref:data/ef-mvc/intro) é um excelente acompanhamento para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="0383f-213">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0383f-214">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="0383f-214">Additional resources</span></span>

* [<span data-ttu-id="0383f-215">Carregamentos de arquivos no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0383f-215">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="0383f-216">IFormFile</span><span class="sxs-lookup"><span data-stu-id="0383f-216">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="0383f-217">Anterior: validação</span><span class="sxs-lookup"><span data-stu-id="0383f-217">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
