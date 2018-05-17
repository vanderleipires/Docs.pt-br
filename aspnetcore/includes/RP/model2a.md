<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="6ed1d-101">Adicionar uma cadeia de conexão de banco de dados</span><span class="sxs-lookup"><span data-stu-id="6ed1d-101">Add a database connection string</span></span>

<span data-ttu-id="6ed1d-102">Adicione uma cadeia de conexão ao arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6ed1d-102">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="6ed1d-103">Registrar o contexto do banco de dados</span><span class="sxs-lookup"><span data-stu-id="6ed1d-103">Register the database context</span></span>

<span data-ttu-id="6ed1d-104">Registre o contexto do banco de dados com o contêiner de [injeção de dependência](xref:fundamentals/dependency-injection) no arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="6ed1d-104">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>