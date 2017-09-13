---
title: Migrating to ASP.NET Core 2.0
date: 2017-09-13 13:18:05
tags: ASP.NET Core
---


# Migrating to ASP.NET Core 2.0

总结下从ASP.NET迁移到ASP.NET Core 2.0要注意的地方.

## 项目文件结构的改变 
项目文件 .csproj 格式在ASP.NET Core 里面将更加简单， 主要体现在以下几个方面:
* 不需要显示标记项目里面包含了哪些文件.
E.g. 以前会在项目文件里面说明需要compile的文件
```xml
    <Compile Include="Properties\AssemblyInfo.cs" />
```
    Comment: 以后再也不用担心加了文件但是忘记把.csproj也check in导致找不到文件的情况了。 感觉应该是扫描目录下面的文件？ 那么假如某些文件不想include进proj呢？ 很简单只需要 
```xml
    <Compile Remove="foo.cs" /> 
```

* 不需要基于GUID来引用其它项目.
```xml
    <ItemGroup>
        <ProjectReference Include="..\Parent\AnotherProj.csproj" />
    </ItemGroup>
```

* 项目文件以前需要unload然后才能编辑，现在可以直接右键编辑.

## 替代Global.asax 
以前会使用Global.asax文件来作为ASP.NET应用的入口，使用这个文件来进行 route configuration, area registration;

现在使用 Startup.cs 文件作为入口，来configure services and request pipeline.
ASP.NET Core 不依赖于OWIN来处理entry，而是通过*Program.cs* 里面的Main方法来加载Startup;
```cs
    WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
```

Startup里面的Congiure方法可以配置requset pipeline, 比如下面的例子配置静态文件服务；

## 提供静态文件服务
在ASP.NET中， 静态文件可以存储在多个目录然后在视图里面被引用；
在ASP.NET Core，静态文件默认放在wwwroot下面，可以通过调用 `UseStaticFiles` 方法来加载这些静态文件进入request pipeline；

```cs
    public void Configure(IApplicationBuilder app)
    {
       app.UseStaticFiles();
    }
```

## 保存settings配置信息
以前的习惯做法是把配置信息作为key-value键值对放在*Web.config*的`<appSettings>`里面
```xml
    <appSettings>
        <add key="UserName" value="User" />
    </appSettings>
```
然后通过`ConfigurationManager.AppSettings`来获取配置信息:
```cs
    string userName = System.Web.Configuration.ConfigurationManager.AppSettings["UserName"];
```

在ASP.NET Core中, 默认使用appsettings.json来存储configuration data； 下面是一个sample：
```json
{
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  },
  // Name of section is your choice
  "AppConfiguration": {
    "UserName": "UserName"
  }
}
```
在Startup.cs里面会作为`IConfiguration`的一个实例加载这个json文件，代码如下：
```cs
    public Startup(IConfiguration configuration)
    {
        Configuration = configuration;
    }

    public IConfiguration Configuration { get; }
```
获取配置信息的代码：
```cs
string userName = Configuration.GetSection("AppConfiguration")["UserName"];
```

## 原生的依赖注入
依赖注入成为了ASP.NET Core的一部分， 所以可以在*Startup.cs*的ConfigureServices里配置服务:
```cs
    public void ConfigureServices(IServiceCollection services)
    {
        // Add application services.
        services.AddTransient<IProductRepository, ProductRepository>();
    }
```

Reference:
https://docs.microsoft.com/en-us/aspnet/core/migration/proper-to-2x/(https://docs.microsoft.com/en-us/aspnet/core/migration/proper-to-2x/)
