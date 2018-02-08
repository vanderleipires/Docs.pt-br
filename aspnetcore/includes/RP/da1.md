# <a name="update-the-generated-pages"></a><span data-ttu-id="9052c-101">Atualizar as páginas geradas</span><span class="sxs-lookup"><span data-stu-id="9052c-101">Update the generated pages</span></span>

<span data-ttu-id="9052c-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9052c-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9052c-103">Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal.</span><span class="sxs-lookup"><span data-stu-id="9052c-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="9052c-104">Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser **Release Date** (duas palavras).</span><span class="sxs-lookup"><span data-stu-id="9052c-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Aplicativo de filme aberto no Chrome mostrando os dados do filme](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="9052c-106">Atualize o código gerado</span><span class="sxs-lookup"><span data-stu-id="9052c-106">Update the generated code</span></span>

<span data-ttu-id="9052c-107">Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas no seguinte código:</span><span class="sxs-lookup"><span data-stu-id="9052c-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](code/Models/Movie.cs?highlight=2,11-12)]
