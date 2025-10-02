
```cs
private static string _instanceName = "Vsys.Authentication";  
public static void ConfigureRedis(this IServiceCollection services, IConfiguration configuration)  
{  
    // Lokalt er connection string.empty, så vi ikke er avhengige av Redis på egen maskin (ennå).  
    var redisConnectionString = configuration.GetConnectionString("Redis");  
    if (!string.IsNullOrWhiteSpace(redisConnectionString))  
    {        // DataProtection persisteres til Redis, for å støtte containere.  
        var connection = ConnectionMultiplexer.Connect(redisConnectionString);  
        services.AddDataProtection()  
            .SetApplicationName(_instanceName)  
            .PersistKeysToStackExchangeRedis(connection, new RedisKey($"{_instanceName}.DataProtectionKeys"));  
                services.AddSingleton<IConnectionMultiplexer>(connection);  
  
        // DistributedCache persisteres til Redis, for å støtte containere.  
        services.AddStackExchangeRedisCache(options =>  
        {  
            options.Configuration = redisConnectionString;  
            options.InstanceName = $"{_instanceName}."; // Prefix for alle keys  
        });  
    }}
    
    
    l
public static void ConfigureRateLimiting(this IServiceCollection services, IConfiguration configuration)  
{  
    services.Configure<RateLimitConfig>(configuration.GetSection(nameof(RateLimitConfig)));  
    services.AddTransient<RateLimitConfig>();  
    services.AddSingleton<IRedisService>(sp => new RedisService(sp.GetRequiredService<IConnectionMultiplexer>(),$"{_instanceName}.") );  
}
```