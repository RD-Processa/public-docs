La clase `ServiceController` expone una interfaz fluent que permite establecer la configuración del servicio de Windows y de los controladores http.

1) Cree un proyecto de aplicación de consola (.NET Framework).
2) Instale el paquete Nuget `'Processa.Services.Merlin.Tracing.Core'`

Ejemplo de uso:

**Program.cs**

```c#
ServiceController
    .Setup()
    .WindowsService<MyService>()
    .AddController<MyController>()
    .Then()
    .Run();
```

Para este ejemplo implmente la clase `MyService` heredando de `ServiceHostBase`y `MyController` de `HttpControllerBase`.

**MyService.cs**
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

**MyController.cs**
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

3) Compile su proyecto.
4) Abra una ventana de línea de comandos (como Administrador) en la carpeta bin del proyecto.
5) Registre el ejecutable como un servicio de Windows (MyFile.exe install)
6) Inicie el servicio de Windows (MyFile.exe start)
7) Compruebe el endpoint http enviado una solictud POST a la url http://localhost:3000/api/demo
8) Detenga el servicio (MyFile.exe stop)
9) Desinstale el servicio (MyFile.exe uninstall)
