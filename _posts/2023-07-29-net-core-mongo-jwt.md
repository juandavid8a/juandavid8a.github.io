---
layout: post
title:  "Cómo usar EntityUser y JWT con MongoDB"
description: "Cómo usar EntityUser y JWT con MongoDB"
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/EbKw0Dcaf6o
---
Codigo paso a paso para integrar EntityUser y JWT con MongoDB.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

Instalamos paquetes:
```C#
  AspNetCore.Identity.MongoDbCore
  MongoDB.Bson
  MongoDB.Driver
  Microsoft.AspNetCore.Authentication.JwtBearer
```

Ingresamos al appsettings:
```C#
    ,
  "MongoDbSettings": {
    "ConnectionString": "url",
    "DatabaseName": "namestring"
  }
```

Creamos ApplicationUser:
```C#
    [CollectionName("users")]
    public class ApplicationUser : MongoIdentityUser<Guid>
    {
        public string FullName { get; set; } = string.Empty;
    }
```

Creamos ApplicationRole:
```C#
    [CollectionName("roles")]
    public class ApplicationRole : MongoIdentityRole<Guid>
    {
        
    }
```

Agregamos a Program.cs:
```C#
//Mongo
builder.Services.Configure<MongoDbSettings>(builder.Configuration.GetSection(nameof(MongoDbSettings)));
BsonSerializer.RegisterSerializer(new GuidSerializer(MongoDB.Bson.BsonType.String));
BsonSerializer.RegisterSerializer(new DateTimeSerializer(MongoDB.Bson.BsonType.String));
BsonSerializer.RegisterSerializer(new DateTimeOffsetSerializer(MongoDB.Bson.BsonType.String));

//add mongoIdentityConfiguration...
var mongoDbIdentityConfig = (MongoDbSettings)builder.Configuration.GetSection(nameof(MongoDbSettings)),
    IdentityOptionsAction = options =>
    {
        options.Password.RequireDigit = false;
        options.Password.RequiredLength = 8;
        options.Password.RequireNonAlphanumeric = true;
        options.Password.RequireLowercase = false;

        //lockout
        options.Lockout.DefaultLockoutTimeSpan = TimeSpan.FromMinutes(10);
        options.Lockout.MaxFailedAccessAttempts = 5;
        options.User.RequireUniqueEmail = true;
    }
};

builder.Services.ConfigureMongoDbIdentity<ApplicationUser, ApplicationRole, Guid>(mongoDbIdentityConfig)
    .AddUserManager<UserManager<ApplicationUser>>()
    .AddSignInManager<SignInManager<ApplicationUser>>()
    .AddRoleManager<RoleManager<ApplicationRole>>()
    .AddDefaultTokenProviders();

builder.Services.AddAuthentication(x =>
{
    x.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    x.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
}).AddJwtBearer(x =>
{
    x.RequireHttpsMetadata = true;
    x.SaveToken = true;
    x.TokenValidationParameters = new TokenValidationParameters
    {
        ValidateIssuerSigningKey = true,
        ValidateIssuer = true,
        ValidateAudience = true,
        ValidateLifetime = true,
        ValidIssuer = "https://localhost:5001",
        ValidAudience = "https://localhost:5001",
        IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("1swek3u4uo2u4a6e")),
        ClockSkew = TimeSpan.Zero
    };
});

builder.Services.AddSwaggerGen(setup =>
{
    // Include 'SecurityScheme' to use JWT Authentication
    var jwtSecurityScheme = new OpenApiSecurityScheme
    {
        BearerFormat = "JWT",
        Name = "JWT Authentication",
        In = ParameterLocation.Header,
        Type = SecuritySchemeType.Http,
        Scheme = JwtBearerDefaults.AuthenticationScheme,
        Description = "Put **_ONLY_** your JWT Bearer token on textbox below!",

        Reference = new OpenApiReference
        {
            Id = JwtBearerDefaults.AuthenticationScheme,
            Type = ReferenceType.SecurityScheme
        }
    };
    setup.AddSecurityDefinition(jwtSecurityScheme.Reference.Id, jwtSecurityScheme);
    setup.AddSecurityRequirement(new OpenApiSecurityRequirement
    {
        { jwtSecurityScheme, Array.Empty<string>() }
    });
});
```

Creamos LoginRequest Dto:
```C#
[Required, EmailAddress]
public string Email { get; set; } = string.Empty;
[Required, DataType(DataType.Password)]
public string Password { get; set; } = string.Empty;
```

Creamos RegisterRequest Dto:
```C#
[Required, EmailAddress]
public string Email { get; set; } = string.Empty;
public string Username { get; set; } = string.Empty;
[Required]
public string FullName { get; set; } = string.Empty;

[Required, DataType(DataType.Password)]
public string Password { get; set; } = string.Empty;
[Required, DataType(DataType.Password), Compare(nameof(Password), ErrorMessage = "Passwords do not match")]
public string ConfirmPassword { get; set; } = string.Empty;
```

Creamos CreateRoleRequest Dto:
```C#
public string Role { get; set; } = string.Empty;
```

Creamos LoginResponse Dto:
```C#
public bool Success { get; set; }
public string AccessToken { get; set; } = string.Empty;
public string Email { get; set; } = string.Empty;
public string UserId { get; set; } = string.Empty;
public string Message { get; set; } = string.Empty;
```

Creamos RegisterResponse Dto:
```C#
public string Message { get; set; } = string.Empty;
public bool Success { get; set; }
```

Creamos AuthenticationController:
```C#
[ApiController]
[Route("api/v1/authenticate")]
public class AuthenticationController : ControllerBase
{
    private readonly UserManager<ApplicationUser> _userManager;
    private readonly RoleManager<ApplicationRole> _roleManager;

    public AuthenticationController(UserManager<ApplicationUser> userManager, RoleManager<ApplicationRole> roleManager)
    {
        _userManager = userManager;
        _roleManager = roleManager;
    }

    [HttpPost]
    [Route("roles/add")]
    public async Task<IActionResult> CreateRole([FromBody] CreateRoleRequest request)
    {
        var appRole = new ApplicationRole { Name = request.Role };
        var createRole = await _roleManager.CreateAsync(appRole);
        return Ok(new { message = "role created succesfully" });
    }

    [HttpPost]
    [Route("register")]
    public async Task<IActionResult> Register([FromBody] RegisterRequest request)
    {
        var result = await RegisterAsync(request);
        return result.Success ? Ok(result) : BadRequest(result.Message);
    }

    private async Task<RegisterResponse> RegisterAsync(RegisterRequest request)
    {
        try
        {
            var userExists = await _userManager.FindByEmailAsync(request.Email);
            if(userExists != null) return new RegisterResponse { Message = "User already exists", Success = false };

            //if we get here, no user with this email..

            userExists = new ApplicationUser
            {
                FullName = request.FullName,
                Email = request.Email,
                ConcurrencyStamp = Guid.NewGuid().ToString(),
                UserName = request.Email,

            };
            var createUserResult = await _userManager.CreateAsync(userExists, request.Password);
            if(!createUserResult.Succeeded) return new RegisterResponse { Message = $"Create user failed {createUserResult?.Errors?.First()?.Description}", Success = false };
            //user is created...
            //then add user to a role...
            var addUserToRoleResult = await _userManager.AddToRoleAsync(userExists, "USER");
            if(!addUserToRoleResult.Succeeded) return new RegisterResponse { Message = $"Create user succeeded but could not add user to role {addUserToRoleResult?.Errors?.First()?.Description}", Success = false };

            //all is still well..
            return new RegisterResponse
            {
                Success = true,
                Message = "User registered successfully"
            };
        }
        catch (Exception ex)
        {
            return new RegisterResponse { Message = ex.Message, Success = false };
        }
    }

    [HttpPost]
    [Route("login")]
    [ProducesResponseType((int) HttpStatusCode.OK , Type = typeof(LoginResponse))]
    public async Task<IActionResult> Login([FromBody] LoginRequest request)
    {
        var result = await LoginAsync(request);
        return result.Success ? Ok(result) : BadRequest(result.Message);
    }

    private async Task<LoginResponse> LoginAsync(LoginRequest request)
    {
        try
        {
            var user = await _userManager.FindByEmailAsync(request.Email);
            if (user is null) return new LoginResponse { Message = "Invalid email/password", Success = false };

            //all is well if ew reach here
            var claims = new List<Claim>
        {
            new Claim(JwtRegisteredClaimNames.Sub, user.Id.ToString()),
            new Claim(ClaimTypes.Name, user.UserName),
            new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
            new Claim(ClaimTypes.NameIdentifier, user.Id.ToString())
        };
            var roles = await _userManager.GetRolesAsync(user);
            var roleClaims = roles.Select(x => new Claim(ClaimTypes.Role, x));
            claims.AddRange(roleClaims);

            var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("1swek3u4uo2u4a6e"));
            var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);
            var expires = DateTime.Now.AddMinutes(30);

            var token = new JwtSecurityToken(
                issuer: "https://localhost:5001",
                audience: "https://localhost:5001",
                claims: claims,
                expires: expires,
                signingCredentials: creds
                );

            return new LoginResponse
            {
                AccessToken = new JwtSecurityTokenHandler().WriteToken(token),
                Message = "Login Successful",
                Email = user?.Email,
                Success = true,
                UserId = user?.Id.ToString()
            };
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
            return new LoginResponse { Success = false, Message = ex.Message };
        }
    }
}
```

Agregamos [Authorize]
