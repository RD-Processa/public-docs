La clase `ServiceController` expone una interfaz fluent que permite establecer la configuración del servicio de Windows y de los controladores http.

Ejemplo de uso:

```c#
ServiceController
    .Setup()
    .WindowsService<MyService>()
    .AddController<MyController>()
    .Then()
    .Run();
```

Para este ejemplo la clase `MyService` debe heredar de `ServiceHostBase`y `MyController` de `HttpControllerBase`.

```c#
public class MyService : ServiceHostBase
{
	public MyService()
	{
		this.Name = "Demo"; // Nombre del servicio Windows
		this.Account = StartupAccount.LocalService; // Cuenta con la que se debe ejecutar el servicio de Windows.
		this.Url = new ServiceUrl("http://localhost:3000"); // Url en la que se expone el endpoint http del servicio.
		this.Description = "Lorem Ipsum" //Opcional. Si no se establece se utiliza Name.
	}

	public override string Name { get; }
	public override string Description { get; }
	public override StartupAccount Account { get; }
	public override ServiceUrl Url { get; }
}
```

```c#
public class MyController : HttpControllerBase
{
	// LLame al constructor de la clase base con el nombre de la operación
	// En este ejemplo para invicar la operación debería enviar un POST a http://localhost:3000/api/demo
	public MyController() : base("demo")
	{            
	}

	// Método que hace el trabajo y retorna la respuesta.
	protected override Response GetResponse()
	{
		// La propiedad Payload contiene los datos del evento (Dictionary<string, object>)
		var payload = this.Payload;
		
		// La clase ServiceLocator.Logging expone métodos para escribir en los archivos de logs.
		ServiceLocator.Logging.Info(payload);
			
		// Implemente el trabajo necesario para la operación/suscriptor


		// Retorne el resultado de la operación.
		// Se puede hacer de varias formas.
		// 1) Un miembro de la enumeración HttpStatusCode.
		// 2) Una clase que herede de Response como JsonResponse o TextResponse
		// 3) Un entero que represente un valor de HttpStatusCode
		return HttpStatusCode.OK;
	}
}
```
