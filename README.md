# Portal AdventureWorks

Aplicacion ASPNET CORE web, que utiliza la plantilla de autenticacion por cuentas individuales y el patron MVC(Modelo-Vista-Controlador) con migraciones a base de datos SQL SERVER.

## Primeros pasos

### Creacion del proyecto

Considerando que se tiene instalado `dotnet CLI`, se ejecuta el siguiente comando 

```ps
dotnet new mvc --auth Individual -o proyecto
```

Colocar HTTPS en el ambiente de desarrollo

```ps
dotnet dev-certs https --trust
```

**Ejecutar el proyecto**

```ps
dotnet run
```

### Instalar Entity Framework

```ps
dotnet tool install --global dotnet-ef

dotnet tool install --global dotnet-ef --version 7.0.17
```

```ps
dotnet add package Microsoft.EntityFrameworkCore.Design
```

### Configuracion de base de datos

> Por defecto el proyecto viene configurado para `SQLite`, necesitamos colocar los paquetes correctos y string de conexion para aplicar las migraciones a la base de datos

**Para SQL Server**

```ps
dotnet add package Microsoft.EntityFrameworkCore.SqlServer

dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 7.0.17
```

Abrir `Program.cs` y cambiar la configuracion para usar sqlserver:

```c#
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlite(connectionString));
    // change to
    options.UseSqlServer(connectionString));
```

**Configurar las migraciones para evitar problemas con Primary Keys**

`CreateIdentitySchema.cs`

Configurar las primary keys de tipo Int y String

```c#
// Int primary keys should be use the sqlserver identity
Id = table.Column<int>(type: "int", nullable: false)
    .Annotation("SqlServer:Identity", "1, 1"),

// String primary keys it should be setted to maxlength value
Id = table.Column<string>(nullable: false,maxLength: 128),
```

**Colocar el `StringConnection` correcto**

`appsettings.json`

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=PortalAspnetCore;User Id=zub;Password=root;MultipleActiveResultSets=true;Trusted_Connection=True;TrustServerCertificate=True;"
  },
}
```

### Creacion de base de datos

**Aplicar las migraciones**

```ps
dotnet ef database update
```

> Si tenemos un error/warning de cambios pendientes en el modelo, se puede crear una nueva migracion y volver a ejecutar el comando anterior `dotnet ef migrations add sqlserverconfig`


# [READMORE](./docs/)