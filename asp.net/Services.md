# Services

## Singleton

Created once and shared across the entire app.

```csharp
builder.Services.AddSingleton<IMyService, MyService>();
```

## Scoped

Created once per HTTP request.

```csharp
builder.Services.AddScoped<IMyService, MyService>();
```

## Transient

A new instance every time it’s requested.

```csharp
services.AddTransient<IMyService, MyService>();
```

