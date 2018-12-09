---
title: Carregar arquivos para uma Página Razor no ASP.NET Core
author: guardrex
description: Saiba como carregar arquivos em uma página Razor no ASP.NET Core usando a classe FileUpload.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 11/10/2018
ms.custom: seodec18
uid: razor-pages/upload-files
ms.openlocfilehash: 80929c6c1a95b46b942958def1540ac8ed5abc81
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121395"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="cbee5-103">Carregar arquivos para uma Página Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cbee5-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="cbee5-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cbee5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="cbee5-105">Este tópico tem como base o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) em <xref:tutorials/razor-pages/razor-pages-start>.</span><span class="sxs-lookup"><span data-stu-id="cbee5-105">This topic builds upon the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) in <xref:tutorials/razor-pages/razor-pages-start>.</span></span>

<span data-ttu-id="cbee5-106">Este tópico mostra como usar a associação de modelos simples para carregar arquivos, o que funciona bem para carregar arquivos pequenos.</span><span class="sxs-lookup"><span data-stu-id="cbee5-106">This topic shows how to use simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="cbee5-107">Para obter informações sobre a transmissão de arquivos grandes, consulte [Carregando arquivos grandes com streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="cbee5-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="cbee5-108">Nas etapas a seguir, um recurso de upload de arquivo da agenda de filmes será adicionado ao aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="cbee5-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="cbee5-109">Um agendamento de filmes é representado por uma classe `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="cbee5-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="cbee5-110">A classe inclui duas versões do agendamento.</span><span class="sxs-lookup"><span data-stu-id="cbee5-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="cbee5-111">Uma versão é fornecida aos clientes, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="cbee5-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="cbee5-112">A outra versão é usada para os funcionários da empresa, `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="cbee5-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="cbee5-113">Cada versão é carregada como um arquivo separado.</span><span class="sxs-lookup"><span data-stu-id="cbee5-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="cbee5-114">O tutorial demonstra como executar dois carregamentos de arquivos de uma página com um único POST para o servidor.</span><span class="sxs-lookup"><span data-stu-id="cbee5-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

<span data-ttu-id="cbee5-115">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/upload-files/samples) ([como baixar](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cbee5-115">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/upload-files/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="cbee5-116">Considerações sobre segurança</span><span class="sxs-lookup"><span data-stu-id="cbee5-116">Security considerations</span></span>

<span data-ttu-id="cbee5-117">É preciso ter cuidado ao fornecer aos usuários a possibilidade de carregar arquivos em um servidor.</span><span class="sxs-lookup"><span data-stu-id="cbee5-117">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="cbee5-118">Os invasores podem executar uma [negação de serviço](/windows-hardware/drivers/ifs/denial-of-service) e outros ataques em um sistema.</span><span class="sxs-lookup"><span data-stu-id="cbee5-118">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="cbee5-119">Algumas etapas de segurança que reduzem a probabilidade de um ataque bem-sucedido são:</span><span class="sxs-lookup"><span data-stu-id="cbee5-119">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="cbee5-120">Fazer o upload de arquivos para uma área de upload de arquivos dedicada no sistema, o que facilita a aplicação de medidas de segurança sobre o conteúdo carregado.</span><span class="sxs-lookup"><span data-stu-id="cbee5-120">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="cbee5-121">Ao permitir os uploads de arquivos, certifique-se de que as permissões de execução estejam desabilitadas no local do upload.</span><span class="sxs-lookup"><span data-stu-id="cbee5-121">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="cbee5-122">Use um nome de arquivo seguro determinado pelo aplicativo, não da entrada do usuário ou o nome do arquivo carregado.</span><span class="sxs-lookup"><span data-stu-id="cbee5-122">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="cbee5-123">Permitir somente um conjunto específico de extensões de arquivo aprovadas.</span><span class="sxs-lookup"><span data-stu-id="cbee5-123">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="cbee5-124">Certifique-se de que as verificações do lado do cliente sejam realizadas no servidor.</span><span class="sxs-lookup"><span data-stu-id="cbee5-124">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="cbee5-125">As verificações do lado do cliente são fáceis de contornar.</span><span class="sxs-lookup"><span data-stu-id="cbee5-125">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="cbee5-126">Verifique o tamanho do upload e evite uploads maiores do que o esperado.</span><span class="sxs-lookup"><span data-stu-id="cbee5-126">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="cbee5-127">Execute um verificador de vírus/malware no conteúdo carregado.</span><span class="sxs-lookup"><span data-stu-id="cbee5-127">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="cbee5-128">Carregar códigos mal-intencionados em um sistema é frequentemente a primeira etapa para executar o código que pode:</span><span class="sxs-lookup"><span data-stu-id="cbee5-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="cbee5-129">Tomar o controle total de um sistema.</span><span class="sxs-lookup"><span data-stu-id="cbee5-129">Completely takeover a system.</span></span>
> * <span data-ttu-id="cbee5-130">Sobrecarregar um sistema, fazendo com que ele falhe completamente.</span><span class="sxs-lookup"><span data-stu-id="cbee5-130">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="cbee5-131">Comprometer dados do sistema ou de usuários.</span><span class="sxs-lookup"><span data-stu-id="cbee5-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="cbee5-132">Aplicar pichações a uma interface pública.</span><span class="sxs-lookup"><span data-stu-id="cbee5-132">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="cbee5-133">Adicionar uma classe FileUpload</span><span class="sxs-lookup"><span data-stu-id="cbee5-133">Add a FileUpload class</span></span>

<span data-ttu-id="cbee5-134">Criar uma Página Razor para lidar com um par de carregamentos de arquivos.</span><span class="sxs-lookup"><span data-stu-id="cbee5-134">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="cbee5-135">Adicione uma classe `FileUpload`, que é vinculada à página para obter os dados do agendamento.</span><span class="sxs-lookup"><span data-stu-id="cbee5-135">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="cbee5-136">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="cbee5-136">Right click the *Models* folder.</span></span> <span data-ttu-id="cbee5-137">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="cbee5-137">Select **Add** > **Class**.</span></span> <span data-ttu-id="cbee5-138">Nomeie a classe **FileUpload** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="cbee5-138">Name the class **FileUpload** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

<span data-ttu-id="cbee5-139">A classe tem uma propriedade para o título do agendamento e uma propriedade para cada uma das duas versões do agendamento.</span><span class="sxs-lookup"><span data-stu-id="cbee5-139">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="cbee5-140">Todas as três propriedades são necessárias e o título deve ter 3-60 caracteres.</span><span class="sxs-lookup"><span data-stu-id="cbee5-140">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="cbee5-141">Adicionar um método auxiliar para carregar arquivos</span><span class="sxs-lookup"><span data-stu-id="cbee5-141">Add a helper method to upload files</span></span>

<span data-ttu-id="cbee5-142">Para evitar duplicação de código para processar arquivos do agendamento carregados, primeiro, adicione um método auxiliar estático.</span><span class="sxs-lookup"><span data-stu-id="cbee5-142">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="cbee5-143">Crie uma pasta de *Utilitários* no aplicativo e adicione um arquivo *FileHelpers.cs* com o seguinte conteúdo.</span><span class="sxs-lookup"><span data-stu-id="cbee5-143">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="cbee5-144">O método auxiliar, `ProcessFormFile`, usa um [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) e [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) e retorna uma cadeia de caracteres que contém o tamanho e o conteúdo do arquivo.</span><span class="sxs-lookup"><span data-stu-id="cbee5-144">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="cbee5-145">O comprimento e o tipo de conteúdo são verificados.</span><span class="sxs-lookup"><span data-stu-id="cbee5-145">The content type and length are checked.</span></span> <span data-ttu-id="cbee5-146">Se o arquivo não passar em uma verificação de validação, um erro será adicionado ao `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="cbee5-146">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a><span data-ttu-id="cbee5-147">Salvar o arquivo no disco</span><span class="sxs-lookup"><span data-stu-id="cbee5-147">Save the file to disk</span></span>

<span data-ttu-id="cbee5-148">O aplicativo de exemplo salva os arquivos carregados em campos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cbee5-148">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="cbee5-149">Para salvar um arquivo no disco, use um [FileStream](/dotnet/api/system.io.filestream).</span><span class="sxs-lookup"><span data-stu-id="cbee5-149">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="cbee5-150">O exemplo a seguir copia um arquivo mantido por `FileUpload.UploadPublicSchedule` para um `FileStream` em um método `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="cbee5-150">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="cbee5-151">O `FileStream` grava o arquivo no disco no `<PATH-AND-FILE-NAME>` fornecido:</span><span class="sxs-lookup"><span data-stu-id="cbee5-151">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Perform an initial check to catch FileUpload class attribute violations.
    if (!ModelState.IsValid)
    {
        return Page();
    }

    var filePath = "<PATH-AND-FILE-NAME>";

    using (var fileStream = new FileStream(filePath, FileMode.Create))
    {
        await FileUpload.UploadPublicSchedule.CopyToAsync(fileStream);
    }

    return RedirectToPage("./Index");
}
```

<span data-ttu-id="cbee5-152">O processo de trabalho deve ter permissões de gravação para o local especificado por `filePath`.</span><span class="sxs-lookup"><span data-stu-id="cbee5-152">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="cbee5-153">O `filePath` *precisa* incluir o nome do arquivo.</span><span class="sxs-lookup"><span data-stu-id="cbee5-153">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="cbee5-154">Se o nome do arquivo não for fornecido, uma [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) será gerada no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="cbee5-154">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="cbee5-155">Nunca persista os arquivos carregados na mesma árvore de diretório que o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cbee5-155">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="cbee5-156">O exemplo de código não oferece proteção do lado do servidor contra carregamentos de arquivos mal-intencionados.</span><span class="sxs-lookup"><span data-stu-id="cbee5-156">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="cbee5-157">Para obter informações de como reduzir a área da superfície de ataque ao aceitar arquivos de usuários, confira os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="cbee5-157">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="cbee5-158">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Carregamento de arquivo irrestrito)</span><span class="sxs-lookup"><span data-stu-id="cbee5-158">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="cbee5-159">Segurança do Azure: Verifique se os controles adequados estão em vigor ao aceitar arquivos de usuários</span><span class="sxs-lookup"><span data-stu-id="cbee5-159">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="cbee5-160">Salvar o arquivo no Armazenamento de Blobs do Azure</span><span class="sxs-lookup"><span data-stu-id="cbee5-160">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="cbee5-161">Para carregar o conteúdo do arquivo para o Armazenamento de Blobs do Azure, confira [Introdução ao Armazenamento de Blobs do Azure usando o .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="cbee5-161">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="cbee5-162">O tópico demonstra como usar [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) para salvar um [FileStream](/dotnet/api/system.io.filestream) para armazenamento de blobs.</span><span class="sxs-lookup"><span data-stu-id="cbee5-162">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="cbee5-163">Adicionar a classe de Agendamento</span><span class="sxs-lookup"><span data-stu-id="cbee5-163">Add the Schedule class</span></span>

<span data-ttu-id="cbee5-164">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="cbee5-164">Right click the *Models* folder.</span></span> <span data-ttu-id="cbee5-165">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="cbee5-165">Select **Add** > **Class**.</span></span> <span data-ttu-id="cbee5-166">Nomeie a classe **Agendamento** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="cbee5-166">Name the class **Schedule** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

<span data-ttu-id="cbee5-167">Usa a classe usa os atributos `Display` e `DisplayFormat`, que produzem formatação e títulos fáceis quando os dados de agendamento são renderizados.</span><span class="sxs-lookup"><span data-stu-id="cbee5-167">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a><span data-ttu-id="cbee5-168">Atualizar o RazorPagesMovieContext</span><span class="sxs-lookup"><span data-stu-id="cbee5-168">Update the RazorPagesMovieContext</span></span>

<span data-ttu-id="cbee5-169">Especifique um `DbSet` no `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) para os agendamentos:</span><span class="sxs-lookup"><span data-stu-id="cbee5-169">Specify a `DbSet` in the `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a><span data-ttu-id="cbee5-170">Atualizar o MovieContext</span><span class="sxs-lookup"><span data-stu-id="cbee5-170">Update the MovieContext</span></span>

<span data-ttu-id="cbee5-171">Especifique um `DbSet` no `MovieContext` (*Models/MovieContext.cs*) para os agendamentos:</span><span class="sxs-lookup"><span data-stu-id="cbee5-171">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="cbee5-172">Adicione a tabela de Agendamento ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="cbee5-172">Add the Schedule table to the database</span></span>

<span data-ttu-id="cbee5-173">Abra o Console Gerenciador de pacote (PMC): **Ferramentas** > **Gerenciador de pacote NuGet** > **Console Gerenciador de pacote**.</span><span class="sxs-lookup"><span data-stu-id="cbee5-173">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Menu do PMC](upload-files/_static/pmc.png)

<span data-ttu-id="cbee5-175">No PMC, execute os seguintes comandos.</span><span class="sxs-lookup"><span data-stu-id="cbee5-175">In the PMC, execute the following commands.</span></span> <span data-ttu-id="cbee5-176">Estes comandos adicionam uma tabela `Schedule` ao banco de dados:</span><span class="sxs-lookup"><span data-stu-id="cbee5-176">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="cbee5-177">Adicionar uma página do Razor de upload de arquivo</span><span class="sxs-lookup"><span data-stu-id="cbee5-177">Add a file upload Razor Page</span></span>

<span data-ttu-id="cbee5-178">Na pasta *Páginas*, crie uma pasta *Agendamentos*.</span><span class="sxs-lookup"><span data-stu-id="cbee5-178">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="cbee5-179">Na pasta *Agendamentos*, crie uma página chamada *Index.cshtml* para carregar um agendamento com o seguinte conteúdo:</span><span class="sxs-lookup"><span data-stu-id="cbee5-179">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

<span data-ttu-id="cbee5-180">Cada grupo de formulário inclui um **\<rótulo>** que exibe o nome de cada propriedade de classe.</span><span class="sxs-lookup"><span data-stu-id="cbee5-180">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="cbee5-181">Os atributos `Display` no modelo `FileUpload` fornecem os valores de exibição para os rótulos.</span><span class="sxs-lookup"><span data-stu-id="cbee5-181">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="cbee5-182">Por exemplo, o nome de exibição da propriedade `UploadPublicSchedule` é definido com `[Display(Name="Public Schedule")]` e, portanto, exibe "Agendamento público" no rótulo quando o formulário é renderizado.</span><span class="sxs-lookup"><span data-stu-id="cbee5-182">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="cbee5-183">Cada grupo de formulário inclui uma validação **\<span>**.</span><span class="sxs-lookup"><span data-stu-id="cbee5-183">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="cbee5-184">Se a entrada do usuário não atender aos atributos de propriedade definidos na classe `FileUpload` ou se qualquer uma das verificações de validação do arquivo de método `ProcessFormFile` falhar, o modelo não será validado.</span><span class="sxs-lookup"><span data-stu-id="cbee5-184">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="cbee5-185">Quando a validação do modelo falha, uma mensagem de validação útil é renderizada para o usuário.</span><span class="sxs-lookup"><span data-stu-id="cbee5-185">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="cbee5-186">Por exemplo, a propriedade `Title` é anotada com `[Required]` e `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="cbee5-186">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="cbee5-187">Se o usuário não fornecer um título, ele receberá uma mensagem indicando que um valor é necessário.</span><span class="sxs-lookup"><span data-stu-id="cbee5-187">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="cbee5-188">Se o usuário inserir um valor com menos de três caracteres ou mais de sessenta, ele receberá uma mensagem indicando que o valor tem um comprimento incorreto.</span><span class="sxs-lookup"><span data-stu-id="cbee5-188">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="cbee5-189">Se um arquivo que não tem nenhum conteúdo for fornecido, uma mensagem aparecerá indicando que o arquivo está vazio.</span><span class="sxs-lookup"><span data-stu-id="cbee5-189">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="cbee5-190">Adicionar o modelo de página</span><span class="sxs-lookup"><span data-stu-id="cbee5-190">Add the page model</span></span>

<span data-ttu-id="cbee5-191">Adicione o modelo de página (*Index.cshtml.cs*) à pasta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="cbee5-191">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

<span data-ttu-id="cbee5-192">O modelo de página (`IndexModel` no *Index.cshtml.cs*) associa a classe `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="cbee5-192">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="cbee5-193">O modelo também usa uma lista dos agendamentos (`IList<Schedule>`) para exibir os agendamentos armazenados no banco de dados na página:</span><span class="sxs-lookup"><span data-stu-id="cbee5-193">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="cbee5-194">Quando a página for carregada com `OnGetAsync`, `Schedules` é preenchido com o banco de dados e usado para gerar uma tabela HTML de agendamentos carregados:</span><span class="sxs-lookup"><span data-stu-id="cbee5-194">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="cbee5-195">Quando o formulário é enviado para o servidor, o `ModelState` é verificado.</span><span class="sxs-lookup"><span data-stu-id="cbee5-195">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="cbee5-196">Se for inválido, `Schedule` é recriado e a página é renderizada com uma ou mais mensagens de validação informando por que a validação de página falhou.</span><span class="sxs-lookup"><span data-stu-id="cbee5-196">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="cbee5-197">Se for válido, as propriedades `FileUpload` serão usadas em *OnPostAsync* para concluir o upload do arquivo para as duas versões do agendamento e criar um novo objeto `Schedule` para armazenar os dados.</span><span class="sxs-lookup"><span data-stu-id="cbee5-197">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="cbee5-198">O agendamento, em seguida, é salvo no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="cbee5-198">The schedule is then saved to the database:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="cbee5-199">Vincular a página do Razor de upload de arquivo</span><span class="sxs-lookup"><span data-stu-id="cbee5-199">Link the file upload Razor Page</span></span>

<span data-ttu-id="cbee5-200">Abra *Pages/Shared/_Layout.cshtml* e adicione um link para a barra de navegação para acessar a página de Agendas:</span><span class="sxs-lookup"><span data-stu-id="cbee5-200">Open *Pages/Shared/_Layout.cshtml* and add a link to the navigation bar to reach the Schedules page:</span></span>

```cshtml
<div class="navbar-collapse collapse">
    <ul class="nav navbar-nav">
        <li><a asp-page="/Index">Home</a></li>
        <li><a asp-page="/Schedules/Index">Schedules</a></li>
        <li><a asp-page="/About">About</a></li>
        <li><a asp-page="/Contact">Contact</a></li>
    </ul>
</div>
```

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="cbee5-201">Adicionar uma página para confirmar a exclusão de agendamento</span><span class="sxs-lookup"><span data-stu-id="cbee5-201">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="cbee5-202">Quando o usuário clica para excluir um agendamento, é oferecida uma oportunidade de cancelar a operação.</span><span class="sxs-lookup"><span data-stu-id="cbee5-202">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="cbee5-203">Adicione uma página de confirmação de exclusão (*Delete.cshtml*) à pasta *Agendamentos*:</span><span class="sxs-lookup"><span data-stu-id="cbee5-203">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

<span data-ttu-id="cbee5-204">O modelo de página (*Delete.cshtml.cs*) carrega um único agendamento identificado por `id` nos dados de rota da solicitação.</span><span class="sxs-lookup"><span data-stu-id="cbee5-204">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="cbee5-205">Adicione o arquivo *Delete.cshtml.cs* à pasta *Agendamentos*:</span><span class="sxs-lookup"><span data-stu-id="cbee5-205">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

<span data-ttu-id="cbee5-206">O método `OnPostAsync` lida com a exclusão do agendamento pelo seu `id`:</span><span class="sxs-lookup"><span data-stu-id="cbee5-206">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

<span data-ttu-id="cbee5-207">Após a exclusão com êxito do agendamento, o `RedirectToPage` envia o usuário para a página *Index.cshtml* dos agendamentos.</span><span class="sxs-lookup"><span data-stu-id="cbee5-207">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="cbee5-208">A página de Razor de agendamentos de trabalho</span><span class="sxs-lookup"><span data-stu-id="cbee5-208">The working Schedules Razor Page</span></span>

<span data-ttu-id="cbee5-209">Quando a página é carregada, os rótulos e entradas para o título do agendamento, o agendamento público e o agendamento privado são renderizados com um botão de envio:</span><span class="sxs-lookup"><span data-stu-id="cbee5-209">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Agenda a página de Razor como visto no carregamento inicial sem erros de validação e campos vazios](upload-files/_static/browser1.png)

<span data-ttu-id="cbee5-211">Selecionar o botão **Upload** sem preencher nenhum dos campos viola os atributos `[Required]` no modelo.</span><span class="sxs-lookup"><span data-stu-id="cbee5-211">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="cbee5-212">O `ModelState` é inválido.</span><span class="sxs-lookup"><span data-stu-id="cbee5-212">The `ModelState` is invalid.</span></span> <span data-ttu-id="cbee5-213">As mensagens de erro de validação são exibidas para o usuário:</span><span class="sxs-lookup"><span data-stu-id="cbee5-213">The validation error messages are displayed to the user:</span></span>

![As mensagens de erro de validação aparecem ao lado de cada controle de entrada](upload-files/_static/browser2.png)

<span data-ttu-id="cbee5-215">Digite duas letras no campo de **Título**.</span><span class="sxs-lookup"><span data-stu-id="cbee5-215">Type two letters into the **Title** field.</span></span> <span data-ttu-id="cbee5-216">A mensagem de validação muda para indicar que o título deve ter entre 3 e 60 caracteres:</span><span class="sxs-lookup"><span data-stu-id="cbee5-216">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Mensagem de validação de título alterada](upload-files/_static/browser3.png)

<span data-ttu-id="cbee5-218">Quando um ou mais agendamentos são carregados, a seção **Agendamentos carregados** renderiza os agendamentos carregados:</span><span class="sxs-lookup"><span data-stu-id="cbee5-218">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabela de agendamentos carregados, mostrando o título de cada agendamento, dados carregados em UTC, tamanho do arquivo de versão pública e tamanho do arquivo de versão privada](upload-files/_static/browser4.png)

<span data-ttu-id="cbee5-220">O usuário pode clicar no link **Excluir** para chegar à exibição de confirmação de exclusão, na qual ele tem a oportunidade de confirmar ou cancelar a operação de exclusão.</span><span class="sxs-lookup"><span data-stu-id="cbee5-220">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="cbee5-221">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="cbee5-221">Troubleshooting</span></span>

<span data-ttu-id="cbee5-222">Para solucionar problemas de informações com `IFormFile` carregar, consulte [carregamentos de arquivos no ASP.NET Core: solução de problemas](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="cbee5-222">For troubleshooting information with `IFormFile` uploading, see [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>
