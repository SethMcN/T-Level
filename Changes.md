#Blazor 

> [!info]
> Razor is a syntax for combining HTML markup with C# code


Open the Counter.razor file in a text editor and note a few interesting lines that render content and make the component's counter feature work.

Components/Pages/Counter.razor:

```
@page "/counter"

<PageTitle>Counter</PageTitle>

<h1>Counter</h1>

<p role="status">Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

The file starts with a line that indicates the component's relative path (/counter):
```
@page "/counter"
```

The title of the page is set by `<PageTitle>` tags:

```
<PageTitle>Counter</PageTitle>
```

Docs: https://learn.microsoft.com/en-gb/aspnet/core/get-started?view=aspnetcore-10.0

---

>[!tip]
>How To delete template app

To delete the template app remove `Counter.razor`, `Error.razor`  and `Weather.razor` 
![[Pasted image 20251125131937.png]]


Remove `NavMenu.razor` and `NavMenu.razor.css`
![[Pasted image 20251125132218.png]]

Then in `MainLayout.razor` remove 
![[Pasted image 20251125132356.png]]

And in `MainLayout.razor.css` remove or comment out
![[Pasted image 20251125132520.png]]

