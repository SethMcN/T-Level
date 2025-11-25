#Blazor 

# Authorization Setup Steps

  

## Overview

Basic protected routes have been added to the Blazor application using cookie-based authentication. Unauthorized users are redirected to a login page.
## Steps Taken

### 1. Added Auhorization Attributes to Pages

Added `@attribute [Authorize]` to all page components to protect routes:

- `Components/Pages/Home.razor`

- `Components/Pages/Availability.razor`

- `Components/Pages/Bookings.razor`

- `Components/Pages/Customers.razor`

- `Components/Pages/Rooms.Razor`


**Example code added to each page:**

```razor

@page "/pagename"

@attribute [Authorize]

  

<PageTitle>Page Title</PageTitle>

```

  

### 2. Updated Program.cs

Configured authentication and authorization services:

- Added cookie authentication with "Cookies" as the default scheme

- Set login path to `/login`

- Added `UseAuthentication()` and `UseAuthorization()` middleware

- Added cascading authentication state

  

**Code added:**

```csharp

using HotelManagment.Components;

  

var builder = WebApplication.CreateBuilder(args);

  

// Add services to the container.

builder.Services.AddRazorComponents()

    .AddInteractiveServerComponents();

  

builder.Services.AddAuthentication("Cookies")

    .AddCookie("Cookies", options =>

    {

        options.LoginPath = "/login";

    });

builder.Services.AddAuthorization();

builder.Services.AddCascadingAuthenticationState();

  

var app = builder.Build();

  

// Configure the HTTP request pipeline.

if (!app.Environment.IsDevelopment())

{

    app.UseExceptionHandler("/Error", createScopeForErrors: true);

    app.UseHsts();

}

  

app.UseHttpsRedirection();

  

app.UseAuthentication();

app.UseAuthorization();

  

app.UseAntiforgery();

  

app.MapStaticAssets();

app.MapRazorComponents<App>()

    .AddInteractiveServerRenderMode();

  

app.Run();

```

  

### 3. Updated Routes.razor

- Changed `RouteView` to `AuthorizeRouteView`

- Added `<NotAuthorized>` block with `<RedirectToLogin />` component

  

**Complete file:**

```razor

<Router AppAssembly="typeof(Program).Assembly">

    <Found Context="routeData">

        <AuthorizeRouteView RouteData="routeData" DefaultLayout="typeof(Layout.MainLayout)">

            <NotAuthorized>

                <RedirectToLogin />

            </NotAuthorized>

        </AuthorizeRouteView>

        <FocusOnNavigate RouteData="routeData" Selector="h1" />

    </Found>

</Router>

  

@code {

    [CascadingParameter]

    private HttpContext? HttpContext { get; set; }

}

```

  

### 4. Created RedirectToLogin Component

Created `Components/RedirectToLogin.razor` to handle redirects to the login page with return URL.

  

**Complete file:**

```razor

@inject NavigationManager Navigation

  

@code {

    protected override void OnInitialized()

    {

        Navigation.NavigateTo($"/login?returnUrl={Uri.EscapeDataString(Navigation.Uri)}", forceLoad: true);

    }

}

```

  

### 5. Created Login Page

Created `Components/Pages/Login.razor` as a placeholder for login functionality.

  

**Complete file:**

```razor

@page "/login"

  

<PageTitle>Login</PageTitle>

  

<h3>Login</h3>

  

<p>Please implement your login functionality here.</p>

<p>After successful login, redirect to @(ReturnUrl ?? "/")</p>

  

@code {

    [SupplyParameterFromQuery]

    public string? ReturnUrl { get; set; }

}

```

  

### 6. Updated _Imports.razor

Added required using directives:

- `@using Microsoft.AspNetCore.Authorization`

- `@using Microsoft.AspNetCore.Components.Authorization`

  

**Code added:**

```razor

@using System.Net.Http

@using System.Net.Http.Json

@using Microsoft.AspNetCore.Components.Forms

@using Microsoft.AspNetCore.Components.Routing

@using Microsoft.AspNetCore.Components.Web

@using static Microsoft.AspNetCore.Components.Web.RenderMode

@using Microsoft.AspNetCore.Components.Web.Virtualization

@using Microsoft.JSInterop

@using HotelManagment

@using HotelManagment.Components

@using Microsoft.AspNetCore.Authorization

@using Microsoft.AspNetCore.Components.Authorization

```

  

## Next Steps

Implement the login functionality in `Components/Pages/Login.razor` to authenticate users and create authentication cookies.

  

**Example login implementation:**

```csharp

@code {

    [SupplyParameterFromQuery]

    public string? ReturnUrl { get; set; }

    private async Task HandleLogin()

    {

        // Create claims for the user

        var claims = new List<Claim>

        {

            new Claim(ClaimTypes.Name, "username"),

            // Add more claims as needed

        };

        var identity = new ClaimsIdentity(claims, "Cookies");

        var principal = new ClaimsPrincipal(identity);

        // Sign in the user

        await HttpContext.SignInAsync("Cookies", principal);

        // Redirect to return URL or home

        Navigation.NavigateTo(ReturnUrl ?? "/", forceLoad: true);

    }

}

```

Docs: https://learn.microsoft.com/en-us/aspnet/core/blazor/security/?view=aspnetcore-10.0&tabs=visual-studio