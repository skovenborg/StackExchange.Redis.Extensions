# Dependency Injection

## Register all the needed part into your DI container

Becoming a super hero is a fairly straight forward process and one step of this process is to use a dependency injection container.

All you need to do is this:

```csharp
/// <summary>
/// Add StackExchange.Redis with its serialization provider.
/// </summary>
/// <param name="services">The service collection.</param>
/// <param name="redisConfiguration">The redis configration.</param>
/// <typeparam name="T">The typof of serializer. <see cref="ISerializer" />.</typeparam>
public static IServiceCollection AddStackExchangeRedisExtensions<T>(this IServiceCollection services, RedisConfiguration redisConfiguration)
    where T : class, ISerializer, new()
{
    services.AddSingleton<IRedisClient, RedisClient>();
    services.AddSingleton<IRedisConnectionPoolManager, RedisConnectionPoolManager>();
    services.AddSingleton<ISerializer, T>();

    services.AddSingleton((provider) =>
    {
        return provider.GetRequiredService<IRedisClient>().GetDbFromConfiguration();
    });

    services.AddSingleton(redisConfiguration);

    return services;
}
```

There are two important things to know in this code block:

* Ho to retrieve an instance of the redis configuration ([here](configuration/) the solution);
* Choise the right serializer ([here](serializers/) few options)
