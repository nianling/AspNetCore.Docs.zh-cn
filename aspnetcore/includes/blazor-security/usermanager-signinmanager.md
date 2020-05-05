## <a name="usermanager-and-signinmanager"></a><span data-ttu-id="e618f-101">UserManager 和提供</span><span class="sxs-lookup"><span data-stu-id="e618f-101">UserManager and SignInManager</span></span>

<span data-ttu-id="e618f-102">在服务器应用需要时设置用户标识符声明类型：</span><span class="sxs-lookup"><span data-stu-id="e618f-102">Set the user identifier claim type when a Server app requires:</span></span>

* <span data-ttu-id="e618f-103"><xref:Microsoft.AspNetCore.Identity.UserManager%601>或<xref:Microsoft.AspNetCore.Identity.SignInManager%601>在 API 终结点中。</span><span class="sxs-lookup"><span data-stu-id="e618f-103"><xref:Microsoft.AspNetCore.Identity.UserManager%601> or <xref:Microsoft.AspNetCore.Identity.SignInManager%601> in an API endpoint.</span></span>
* <span data-ttu-id="e618f-104"><xref:Microsoft.AspNetCore.Identity.IdentityUser>详细信息，如用户的姓名、电子邮件地址或锁定结束时间。</span><span class="sxs-lookup"><span data-stu-id="e618f-104"><xref:Microsoft.AspNetCore.Identity.IdentityUser> details, such as the user's name, email address, or lockout end time.</span></span>

<span data-ttu-id="e618f-105">在 `Startup.ConfigureServices`中：</span><span class="sxs-lookup"><span data-stu-id="e618f-105">In `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<IdentityOptions>(options => 
    options.ClaimsIdentity.UserIdClaimType = ClaimTypes.NameIdentifier);
```

<span data-ttu-id="e618f-106">调用`Get`方法`WeatherForecastController` <xref:Microsoft.AspNetCore.Identity.IdentityUser%601.UserName>时，以下记录：</span><span class="sxs-lookup"><span data-stu-id="e618f-106">The following `WeatherForecastController` logs the <xref:Microsoft.AspNetCore.Identity.IdentityUser%601.UserName> when the `Get` method is called:</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Identity;
using Microsoft.Extensions.Logging;
using {APP NAMESPACE}.Server.Models;
using {APP NAMESPACE}.Shared;

namespace {APP NAMESPACE}.Server.Controllers
{
    [Authorize]
    [ApiController]
    [Route("[controller]")]
    public class WeatherForecastController : ControllerBase
    {
        private readonly UserManager<ApplicationUser> _userManager;

        private static readonly string[] Summaries = new[]
        {
            "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", 
            "Balmy", "Hot", "Sweltering", "Scorching"
        };

        private readonly ILogger<WeatherForecastController> logger;

        public WeatherForecastController(ILogger<WeatherForecastController> logger, 
            UserManager<ApplicationUser> userManager)
        {
            this.logger = logger;
            _userManager = userManager;
        }

        [HttpGet]
        public async Task<IEnumerable<WeatherForecast>> Get()
        {
            var rng = new Random();

            var user = await _userManager.GetUserAsync(User);

            if (user != null)
            {
                logger.LogInformation($"User.Identity.Name: {user.UserName}");
            }

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = DateTime.Now.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = Summaries[rng.Next(Summaries.Length)]
            })
            .ToArray();
        }
    }
}
```