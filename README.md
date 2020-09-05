# Usar controle de Sessão em Asp.NET Core

#### PACKAGE
```
dotnet add package Microsoft.AspNetCore.Session
```

#### Startup.cs
```
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc.Authorization;

public void ConfigureServices(IServiceCollection services)
{
	services.AddDistributedMemoryCache();

            	services.AddSession(options =>
            	{
                	options.IdleTimeout = TimeSpan.FromMinutes(20);
                	options.Cookie.HttpOnly = true;
               	 options.Cookie.IsEssential = true;
            	});
}
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
	app.UseSession();
}
```

#### CONTROLLER
```
using Microsoft.AspNetCore.Http; 
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc.Authorization;

HttpContext.Session.SetInt32("NomeSessao", valorSessao);  
var valor = HttpContext.Session.GetInt32("NomeSessao"); 

```

#### VIEW
```
@using Microsoft.AspNetCore.Http;

if(Context.Session.GetInt32("NomeSessao") == item.Id){
}
```

#### REMOVE
```
HttpContext.Session.Remove("NomeSessao");
```


# Estender uso de controle de Sessão no Repositorio:
#### STARTUP
Adicionar no método _ConfigureServices_:
```
services.AddHttpContextAccessor();
```
#### Repositorio:
```
public class ProdutoRepositorio
{
	private readonly IHttpContextAccessor _httpContextAccessor;
 	public BaseEletronicaRepository(IHttpContextAccessor httpContextAccessor)
	{
		_httpContextAccessor = httpContextAccessor;
	}
	
	public void GravarSession()
        {
            _httpContextAccessor.HttpContext.Session.SetString("NomeSession", "valorSession");
    	}
	
	public void BuscarSession()
        {
            var valorSession = _httpContextAccessor.HttpContext.Session.GetString("NomeSession");
    	}
	
	public void LimparSession()
        {
            _httpContextAccessor.HttpContext.Session.Remove("NomeSession");
   	}
}
```



