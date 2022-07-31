# what-exactly-do-you-want
Why are these guys not able to work what I've given them to.  This is so simple, take a look.

# Why can't you just believe and work on things?
Why do I get slow-workers all the time.  Why do they give us trouble by asking questions instead of "just work on things" to solve the business problem. WHY??

## Why are you even questioning?  If you have these basic/simple questions ask google.com

This is so simple.  Cant you understand and accomplish the task I gave to you?
```
using Microsoft.AspNetCore.Authentication.Cookies;
using Microsoft.OpenApi.Models;
using Serilog;
using SheriffSaaS.Core.Settings;
using SheriffSaaS.Application.CQRS;
using SheriffSaaS.InfraStructure.Extensions;


var builder = WebApplication.CreateBuilder(args);
var services = builder.Services;
var config = builder.Configuration;


#region IOptions<...> Injectables

// Make IOptions<TenantSettings> available
services.Configure<TenantSettings>(config.GetSection(nameof(TenantSettings)));

#endregion

#region Infrastructure

services.AddDbContextWithMigrations(config);

#region CompositeRoot
    services.AddComposites(config);
#endregion

#endregion


#region Application

services.AddCQRSHandlers();

#endregion

#region "ASP.NET Dependecies"
    services.AddHttpContextAccessor();
    services.AddMvc(); // .AddMvc == AddControllersWithView() + AddRazorPages()
    //services.AddControllers();
    //services.AddRazorPages();

    // config.GetSection("Keys");

    services.AddAntiforgery(options => config.Bind("XSRFSettings", options));
#endregion

services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options => config.Bind("CookieSettings", options));

services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "SheriffSaaS", Version = "v1" });
});

// Wait 30 seconds for graceful shutdown.
// builder.Host.ConfigureHostOptions(o => o.ShutdownTimeout = TimeSpan.FromSeconds(30));

builder.Host.UseSerilog((ctx, conf) => conf.ReadFrom.Configuration(ctx.Configuration));

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseHsts();

    app.UseSwagger();
    app.UseSwaggerUI(options =>
    {
        options.SwaggerEndpoint("/swagger/v1/swagger.json", "v1");
        // options.RoutePrefix = string.Empty;
    });

    app.UseHttpsRedirection();

    // app.UseMigrations();
}
else
{
    app.UseExceptionHandler("/Error");
}

app.UseStaticFiles();
//app.UseRouting();

app.UseAuthentication();
// app.UseIdentityServer();
app.UseAuthorization();

app.MapRazorPages();

app.MapDefaultControllerRoute();
//app.MapControllerRoute(
//    name: "default",
//    pattern: "{controller=Home}/{action=Index}/{id?}");

app.MapControllerRoute(
    name: "Stakeholders",
    pattern: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

// Web Api Controllers
app.MapControllers();

// app.UseSecureAdmin();

app.Run();

public partial class Program { }
```
