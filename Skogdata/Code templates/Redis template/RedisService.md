```cs
using StackExchange.Redis;  
  
namespace Vsys.Authentication.Web.Framework.Caching;  
  
public class RedisService : IRedisService  
{  
    private readonly IConnectionMultiplexer _connection;  
    private readonly string _instanceName;  
  
    public RedisService(IConnectionMultiplexer connection, string instanceName)  
    {        _connection = connection;  
        _instanceName = instanceName;  
    }  
    private IDatabase Db => _connection.GetDatabase();  
    private string WithPrefix(string key) => $"{_instanceName}{key}";  
    public async Task<long> IncrementAsync(string key, TimeSpan? ttl = null, bool presistTtl = false)  
    {        long newValue = await Db.StringIncrementAsync(WithPrefix(key));  
        if ((!presistTtl && ttl.HasValue) || (newValue == 1 && ttl.HasValue))  
        {            await Db.KeyExpireAsync(WithPrefix(key), ttl);  
        }        return newValue;  
    }  
    public async Task<long> DecrementAsync(string key, TimeSpan? ttl = null, bool presistTtl = false)  
    {        long newValue = await Db.StringDecrementAsync(WithPrefix(key));  
        if ((!presistTtl && ttl.HasValue) || (newValue == -1 && ttl.HasValue))  
        {            await Db.KeyExpireAsync(WithPrefix(key), ttl);  
        }        return newValue;  
    }    public async Task SetStringAsync(string key, string value, TimeSpan? ttl = null, bool presistTtl = false)  
    {        await Db.StringSetAsync(WithPrefix(key), value, ttl, keepTtl:presistTtl);  
    }  
    public async Task<string?> GetStringAsync(string key)  
    {        var val = await Db.StringGetAsync(WithPrefix(key));  
        return val.HasValue ? val.ToString() : null;  
    }    public async Task SetBytesAsync(string key, byte[] value, TimeSpan? ttl = null, bool presistTtl = false)  
    {        await Db.StringSetAsync(WithPrefix(key), value, ttl, keepTtl:presistTtl);  
        if (!presistTtl && ttl.HasValue)  
        {            await Db.KeyExpireAsync(key, ttl);  
        }    }  
    public async Task<byte[]?> GetBytesAsync(string key)  
    {        var val = await Db.StringGetAsync(WithPrefix(key));  
        return val.HasValue ? (byte[]?)val : null;  
    }  
    public async Task<bool> KeyExistsAsync(string key)  
    {        return await Db.KeyExistsAsync(WithPrefix(key));  
    }  
    public async Task<bool> RemoveKeyAsync(string key)  
    {        return await Db.KeyDeleteAsync(WithPrefix(key));  
    }  
    // --- TTL ---  
    public async Task<TimeSpan?> GetTimeToLiveAsync(string key)  
    {        return await Db.KeyTimeToLiveAsync(WithPrefix(key));  
    }  
    public async Task<bool> SetKeyExpirationAsync(string key, TimeSpan ttl)  
    {        return await Db.KeyExpireAsync(WithPrefix(key), ttl);  
    }    }
```