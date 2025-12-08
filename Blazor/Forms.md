#Blazor

Easy way to make forms:

Import:
``` cs
@using System.ComponentModel.DataAnnotations
@rendermode InteractiveServer
```

Login form example:
``` cs
<EditForm Model="@loginModel" OnValidSubmit="HandleValidSubmit"FormName="LoginForm">

    <input type="hidden" name="formname" value="LoginForm" />
    
    <div class="mb-3">
        <label class="form-label">Username</label>
        <InputText class="form-control" @bind-Value="loginModel.Username" />
        <ValidationMessage For="@(() => loginModel.Username)" />
    </div>

    <div class="mb-3">
        <label class="form-label">Password</label>
        <InputText class="form-control" type="password" @bind-Value="loginModel.Password" />
        <ValidationMessage For="@(() => loginModel.Password)" />
    </div>
    
    <button type="submit" class="btn btn-primary">Login</button>
</EditForm>
```

Key Parts:
username field:

``` cs
<div class="mb-3">
        <label class="form-label">Username</label>
        <InputText class="form-control" @bind-Value="loginModel.Username" />
        <ValidationMessage For="@(() => loginModel.Username)" />
    </div>
```

Password field:
``` cs
    <div class="mb-3">
        <label class="form-label">Password</label>
        <InputText class="form-control" type="password" @bind Value="loginModel.Password" />
        <ValidationMessage For="@(() => loginModel.Password)" />
    </div>
```

Binding to a useable variable:
``` cs 
@bind Value="loginModel.Password"
```

And `loginModel`:
``` cs 
    private class LoginModel
    {
        [Required(ErrorMessage = "Username is required")]
        public string? Username { get; set; }
        
        [Required(ErrorMessage = "Password is required")]
        public string? Password { get; set; }
    }
```