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
		// LLame al método Initialize (para utilizar la instancia de configuración predeterminada) o a la sobrecarga que admite IAppSettings para utilizar una instancia personalizada.
		this.Initialize();
	}

	public override string Name { get; }
	public override string Description { get; }
	public override StartupAccount Account { get; }
	public override ServiceUrl Url { get; }
}
```
La instancia predeterminada de IAppSettings busca las claves de configuración en el archivo app.config del proyecto con los siguientes nombres:

| Key  | Value  |
|:----:|--------|
| ServiceName  | Nombre del servicio  |
| ServiceAccount  |  Cuenta para iniciar el servicio (valores admitidos: `LocalService`, `LocalSystem`, `NetworkService` y `CredentialManager` |
| ServiceDescription  | Descripción del servicio  |
| ServiceUrl  | URL para publicar el servicio  |

Ejemplo del archivo **app.config**

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <appSettings>
    <add key="ServiceName" value="Mi servicio" />
    <add key="ServiceAccount" value="LocalSystem" />
    <add key="ServiceDescription" value="Lorem Ipsum" />
    <add key="ServiceUrl" value="http://url" />
  </appSettings>
</configuration>
```

**MyController.cs**
```c#
public class MyController : HttpControllerBase
{
	// LLame al constructor de la clase base con el nombre de la operación
	// En este ejemplo para invocar la operación debería enviar un POST a http://localhost:3000/api/demo
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
		// Este método se invocará tantas veces como sea necesario hasta que la respuesta sea un HttpStatusCode.OK (200).
		// Valores de retorno admitidos:
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


### ¿Cómo utilizar CredentialManager en el archivo de configuración?
Si se establece el valor de ServiceAccount a CredentialManager, el proceso de instalación del servicio buscará una entrada en el almacén de credenciales del sistema operativo con el mismo nombre del servicio (valor en ServiceName).

### ¿Cómo agregar una entrada en el almacén de credenciales del sistema operativo? 
Puede utilizar el utilitario [cmdkey](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/cmdkey) que viene con el sistema operativo. 

Por ejemplo: 
```text
cmdkey /generic:MyName /user:myuser /pass:mypassword
```
