## <a name="register-the-database-context"></a><span data-ttu-id="dbd0e-101">Registrar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="dbd0e-101">Register the database context</span></span>

<span data-ttu-id="dbd0e-102">Nesta etapa, o contexto do banco de dados é registrado com o contêiner [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="dbd0e-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="dbd0e-103">Os serviços (como o contexto do banco de dados) que são registrados com o contêiner de injeção de dependência (DI) estão disponíveis para os controladores.</span><span class="sxs-lookup"><span data-stu-id="dbd0e-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="dbd0e-104">Registre o contexto de banco de dados com o contêiner de serviço usando o suporte interno para [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="dbd0e-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="dbd0e-105">Substitua o conteúdo do arquivo *Startup.cs* pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="dbd0e-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="dbd0e-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span><span class="sxs-lookup"><span data-stu-id="dbd0e-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="dbd0e-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span><span class="sxs-lookup"><span data-stu-id="dbd0e-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span></span>

::: moniker-end  

<span data-ttu-id="dbd0e-108">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="dbd0e-108">The preceding code:</span></span>

* <span data-ttu-id="dbd0e-109">Remove o código não usado.</span><span class="sxs-lookup"><span data-stu-id="dbd0e-109">Removes the unused code.</span></span>
* <span data-ttu-id="dbd0e-110">Especifica que um banco de dados em memória é injetado no contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="dbd0e-110">Specifies an in-memory database is injected into the service container.</span></span>
