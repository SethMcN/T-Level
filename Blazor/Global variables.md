#Blazor 

Create `/Services/GlobalState.cs`
```
namespace HotelManagment.Services
{
    public static class GlobalState
    {
        public static string Username = "Guest";
        public static bool IsLoggedIn = false;
        public static int CurrentHotelId = 0;
    }
}
```

### Use anywhere:

```
@using HotelManagment.Services

<p>User: @GlobalState.Username</p>
```


Your file structure must look like this:
```
HotelManagment/
 ├─ Services/
 │    └─ GlobalState.cs
 ├─ Pages/
 ├─ Shared/
 ├─ _Imports.razor
```

