```cs
namespace Vsys.Authentication.Web.Framework.Caching;  
  
public interface IRedisService  
{  
    Task<long> IncrementAsync(string key, TimeSpan? ttl = null, bool presistTtl = false);  
    Task<long> DecrementAsync(string key, TimeSpan? ttl = null, bool presistTtl = false);  
    Task SetStringAsync(string key, string value, TimeSpan? ttl = null, bool presistTtl = false);  
    Task<string?> GetStringAsync(string key);  
    Task SetBytesAsync(string key, byte[] value, TimeSpan? ttl = null, bool presistTtl = false);  
    Task<byte[]?> GetBytesAsync(string key);  
    Task<bool> KeyExistsAsync(string key);  
    Task<bool> RemoveKeyAsync(string key);  
    Task<TimeSpan?> GetTimeToLiveAsync(string key);  
    Task<bool> SetKeyExpirationAsync(string key, TimeSpan ttl);  
}
```