---
layout: post
title:  "Cómo instalar EntityUser y JWT NetCore 5"
description: Cómo instalar service EntityUser y JWT en NetCore 5
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/CDEaBtEeVwM
---
Paso a paso para instalar el servicio EntityUser que trae por defecto .Net Core 5 y activar JWT para el login de usuarios desde un front-end

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

- Crear la carpeta Models
- Crear el modelo: ApplicationUser
```C#
public class ApplicationUser : IdentityUser
{
}
```
- Crear un modelo de prueba
- Crear un controlador de prueba para el modelo de prueba
- Renombrar el DbContext por: ApplicationDbContext
- Actualizar el DbContext

```C#
: IdentityDbContext<ApplicationUser>
```

- Actualizar el ConnectionString
- Actualizar dependencias
- Crear migracion
- Actualizar base de datos
- Crear modelo: RegisterModel

```C#
[Required(ErrorMessage ="User Name is required")]
public string Username { get; set; }

[EmailAddress]
[Required(ErrorMessage = "Email is required")]
public string Email { get; set; }

[Required(ErrorMessage = "Password is required")]
public string Password { get; set; }
```

- Crear modelo: LoginModel

```C#
[Required(ErrorMessage = "User Name is required")]
public string Username { get; set; }

[Required(ErrorMessage = "Password is required")]
public string Password { get; set; }
```

- Crear el controlador: AccountController
- Agregar el constructor al controlador

```C#
private readonly UserManager<ApplicationUser> userManager;
private readonly RoleManager<IdentityRole> roleManager;
private readonly IConfiguration _configuration;

public AccountController(UserManager<ApplicationUser> userManager, RoleManager<IdentityRole> roleManager, IConfiguration configuration)
{
    this.userManager = userManager;
    this.roleManager = roleManager;
    _configuration = configuration;
}
```

- Agregar el metodo registrar dentro del controlador

```C#
[HttpPost]
[Route("register")]
public async Task<IActionResult> Register([FromBody] RegisterModel model)
{
    var userExists = await userManager.FindByNameAsync(model.Username);
    if (userExists != null)
        return StatusCode(StatusCodes.Status500InternalServerError, new Response { Status = "Error", Message = "User already exists!" });

    ApplicationUser user = new ApplicationUser()
    {
        Email = model.Email,
        SecurityStamp = Guid.NewGuid().ToString(),
        UserName = model.Username
    };
    var result = await userManager.CreateAsync(user, model.Password);
    if (!result.Succeeded)
        return StatusCode(StatusCodes.Status500InternalServerError, new Response { Status = "Error", Message = "User creation failed! Please check user details and try again." });

    return Ok(new Response { Status = "Success", Message = "User created successfully!" });
}
```

- Agregar en metodo login dentro del controlador

```C#
[HttpPost]
[Route("login")]
public async Task<IActionResult> Login([FromBody] LoginModel model)
{
    var user = await userManager.FindByNameAsync(model.Username);
    if (user != null && await userManager.CheckPasswordAsync(user, model.Password))
    {
        var userRoles = await userManager.GetRolesAsync(user);

        var authClaims = new List<Claim>
        {
            new Claim(ClaimTypes.Name, user.UserName),
            new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
        };

        foreach (var userRole in userRoles)
        {
            authClaims.Add(new Claim(ClaimTypes.Role, userRole));
        }

        var authSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_configuration["JWT:SecretKey"]));

        var token = new JwtSecurityToken(
            issuer: _configuration.GetSection("JWT:ValidIssuer").Value,
            audience: _configuration.GetSection("JWT:ValidAudience").Value,
            expires: DateTime.Now.AddHours(3),
            claims: authClaims,
            signingCredentials: new SigningCredentials(authSigningKey, SecurityAlgorithms.HmacSha256)
        );

        return Ok(new
        {
            token = new JwtSecurityTokenHandler().WriteToken(token),
            expiration = token.ValidTo
        });
    }
    return Unauthorized();
}
```

- Agregamos los servicios en startup

```C#
// For Identity
services.AddIdentity<ApplicationUser, IdentityRole>()
.AddEntityFrameworkStores<ApplicationDbContext>()
.AddDefaultTokenProviders();

// Adding Authentication
services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultScheme = JwtBearerDefaults.AuthenticationScheme;
})

// Adding Jwt Bearer
.AddJwtBearer(options =>
{
    options.SaveToken = true;
    options.RequireHttpsMetadata = false;
    options.TokenValidationParameters = new TokenValidationParameters()
    {
        ValidateIssuer = true,
        ValidateAudience = true,
        ValidAudience = Configuration.GetSection("JWT:ValidAudience").Value,
        ValidIssuer = Configuration.GetSection("JWT:ValidIssuer").Value,
        IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(Configuration.GetSection("JWT:SecretKey").Value))
    };
});
```

- Actualizamos el servicio de Swagger en startup

```C#
services.AddSwaggerGen(swagger =>
{
    //This is to generate the Default UI of Swagger Documentation
    swagger.SwaggerDoc("v1", new OpenApiInfo
    {
        Version = "v1",
        Title = "ASP.NET 5 Web API",
        Description = "Authentication and Authorization in ASP.NET 5 with JWT and Swagger"
    });
    // To Enable authorization using Swagger (JWT)
    swagger.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme()
    {
        Name = "Authorization",
        Type = SecuritySchemeType.ApiKey,
        Scheme = "Bearer",
        BearerFormat = "JWT",
        In = ParameterLocation.Header,
        Description = "Enter 'Bearer' [space] and then your valid token in the text input below.\r\n\r\nExample: \"Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9\"",
    });
    swagger.AddSecurityRequirement(new OpenApiSecurityRequirement
    {
        {
            new OpenApiSecurityScheme
            {
                Reference = new OpenApiReference
                {
                    Type = ReferenceType.SecurityScheme,
                    Id = "Bearer"
                }
            },
            new string[] {}
        }
    });
});
```

- Agregamos las aplicaciones en startup

```C#
app.UseAuthentication();
app.UseAuthorization();
```

- Agregamos al: appsettings

```C#
"JWT": {
  "SecretKey": "LJLKSFJYEWIYEWBBLKASJDASDASDTWEBNVASNBDVCAS",
  "ValidAudience": "https://localhost:44324",
  "ValidIssuer": "https://localhost:44324"
}
```
- Proteger los metodos con: [Authorize]
