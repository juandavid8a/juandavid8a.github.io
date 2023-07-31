---
layout: post
title:  "Cómo integrar Clean Arquitecture a Net Core 7"
description: "Cómo integrar Clean Arquitecture a Net Core 7"
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/EbKw0Dcaf6o
---
Codigo paso a paso para integrar Clean Arquitecture a Net Core 7.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

1. Creamos una Blank Solution

2. Instalamos paquetes:
- Microsoft.Extensions.Options

3. Agregamos proyecto Class Library .API
- Controllers
- Responses
  
4. Agregamos proyecto Class Library .Core
- DTOs
- Entities
- Enumerations
- Interfaces
- Exceptions
- QueryFilters
- Services
  
5. Agregamos proyecto Class Library .Infrastructure
- Data
- Repositories
- Filters
- Mappings
- Validators

6. Reference
- .API = .Core, .Infrastructure
- .Infrastructure = .Core

7. Ingresamos al appsettings:
```C#
,
"MongoDbSettings": {
  "ConnectionString": "url",
  "DatabaseName": "namestring"
}
```

8. Agregamos al program:
```C#
builder.Services.Configure<MongoDbSettings>(builder.Configuration.GetSection(nameof(MongoDbSettings)));
```

9. Creamos IUserRepository y IUserService:
```C#
Task<List<UserEntity>> GetAll();
```

10. Creamos UserRepository:
```C#
public class UserRepository : IUserRepository
{
    private readonly IMongoCollection<UserEntity> _users;
    public UserRepository(IOptions<MongoDbSettings> options)
    {
        var mongoClient = new MongoClient(options.Value.ConnectionString);
        _users = mongoClient.GetDatabase(options.Value.DatabaseName).GetCollection<UserEntity>("users");
    }
    public async Task<List<UserEntity>> GetAll() => await _users.Find(_ => true).ToListAsync();
}
```

11. Creamos UserService:
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

12. Agregamos al program:
```C#
builder.Services.AddSingleton<IUserService, UserService>();
builder.Services.AddSingleton<IUserRepository, UserRepository>();
```


