---
layout: post
title:  "Cómo integrar Clean Arquitecture a Net Core 7"
description: "Cómo integrar Clean Arquitecture a Net Core 7"
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/vItyn5jd-k8
---
Codigo paso a paso para integrar Clean Arquitecture a Net Core 7.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

1. Creamos una Blank Solution

2. Agregamos proyecto API .Api
- Controllers
- Responses
  
3. Agregamos proyecto Class Library .Core
- DTOs
- Entities
- Enumerations
- Interfaces
- Exceptions
- QueryFilters
- Services
  
4. Agregamos proyecto Class Library .Infrastructure
- Data
- Repositories
- Filters
- Mappings
- Validators

5. Reference
- .API = .Core, .Infrastructure
- .Infrastructure = .Core

6. Ingresamos al appsettings:
```C#
,
"MongoDbSettings": {
  "ConnectionString": "url",
  "DatabaseName": "namestring"
}
```

7. Creamos MongoDbSettingsEntity:
```C#
public string ConnectionString { get; set; } = string.Empty;
public string DatabaseName { get; set; } = string.Empty;
```

8. Instalamos paquetes:
```C#
MongoDB.Bson
MongoDB.Driver
Microsoft.Extensions.Options
AspNetCore.Identity.MongoDbCore
```

9. Agregamos al program:
```C#
builder.Services.Configure<MongoDbSettingsEntity>(builder.Configuration.GetSection(nameof(MongoDbSettings)));
```

10. Creamos el UserEntity:
```C#
[BsonIgnoreExtraElements]
public class UserEntity
{
    [BsonId]
    [BsonRepresentation(MongoDB.Bson.BsonType.ObjectId)]
    public string Id { get; set; } = string.Empty;
    public string Username { get; set; } = string.Empty;
    public string Fullmane { get; set; } = string.Empty;
    public string Email { get; set; } = string.Empty;
}
```

11. Creamos IUserRepository y IUserService:
```C#
Task<List<UserEntity>> GetAll();
```

12. Creamos IContext:
```C#
IMongoCollection<UserEntity> Users { get;  }
```

13. Creamos Context:
```C#
public class Context : IContext
{
    private readonly IMongoDatabase _database;
    public Context(IOptions<MongoDbSettingsEntity> options)
    {
        MongoClient _mongoClient = new MongoClient(options.Value.ConnectionString);
        _database = _mongoClient.GetDatabase(options.Value.DatabaseName);
    }
    public IMongoCollection<UserEntity> Users => _database.GetCollection<UserEntity>("users");
}
```

14. Creamos UserRepository:
```C#
public class UserRepository : IUserRepository
{
    private readonly IContext _context;
    public UserRepository(IContext context)
    {
        _context = context;
    }
    public async Task<List<UserEntity>> GetAll() => await _context.Users.Find(_ => true).ToListAsync();
}
```

15. Creamos UserService:
```C#
public class UserService : IUserService
{
    private readonly IUserRepository _userRepository;
    public UserService(IUserRepository userRepository)
    {
        _userRepository = userRepository;
    }
    public Task<List<UserEntity>> GetAll()
    {
        return _userRepository.GetAll();
    }
}
```

16. Creamos UserService:
```C#
public class UserService : IUserService
{
    private readonly IUserRepository _userRepository;
    public UserService(IUserRepository userRepository)
    {
        _userRepository = userRepository;
    }
    public Task<List<UserEntity>> GetAll()
    {
        return _userRepository.GetAll();
    }
}
```

17. Agregamos al program:
```C#
builder.Services.AddSingleton<IUserService, UserService>();
builder.Services.AddSingleton<IUserRepository, UserRepository>();
```

18. Creo UserController:
```C#
[Route("api/[controller]")]
[ApiController]
public class UserController : ControllerBase
{
    private readonly IUserService _userService;
    public UserController(IUserService userService)
    {
        _userService = userService;
    }

    // GET: api/<UserController>
    [HttpGet]
    public async Task<List<UserEntity>> GetAll()
    {
        return await _userService.GetAll();
    }
}
```
