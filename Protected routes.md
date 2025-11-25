#Blazor 

# Protected Routes Setup Guide

This document outlines the steps taken to implement a simple version of protected routes in the Blazor Hotel Management application.
## 1. Enable Authentication Services in `Program.cs`

We added the necessary services to the dependency injection container to support authentication and authorization.

**File:** `Program.cs`

```csharp

// ...existing code...

builder.Services.AddCascadingAuthenticationState();

builder.Services.AddAuthorization();

  

builder.Services.AddRazorComponents()

    .AddInteractiveServerComponents();

// ...existing code...

```
## 2. Add Global Imports in `_Imports.razor`

We added the necessary namespaces so that authentication components and the layout components are available throughout the application.

**File:** `Components/_Imports.razor`

```razor

// ...existing code...

@using HotelManagment

@using HotelManagment.Components

// Added these lines:

@using HotelManagment.Components.Layout

@using Microsoft.AspNetCore.Components.Authorization

```
## 3. Create the Redirect Component
  
We created a simple component that automatically redirects unauthorized users to the login page.

**File:** `Components/Layout/RedirectToLogin.razor`

```razor

@inject NavigationManager Navigation

  

@code {

    protected override void OnInitialized()

    {

        Navigation.NavigateTo("login");

    }

}

```
## 4. Update Routing Logic in `Routes.razor`

We replaced the standard `RouteView` with `AuthorizeRouteView`. This component checks if the user is authorized to view a page. If not, it renders the content inside the `<NotAuthorized>` tag, which we set to use our `RedirectToLogin` component.

**File:** `Components/Routes.razor
`
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

```
## Next Steps:

To fully utilize this setup:

1. **Create a Login Page**: Create `Components/Pages/Login.razor`.

2. **Protect Pages**: Add `@attribute [Authorize]` to the top of any `.razor` page you want to protect.

3. **Implement Authentication State**: You will need to implement a custom `AuthenticationStateProvider` to handle the actual logic of logging a user in and out (managing tokens, cookies, etc.).

Docs: https://learn.microsoft.com/en-us/aspnet/core/blazor/security/?view=aspnetcore-10.0&tabs=visual-studio