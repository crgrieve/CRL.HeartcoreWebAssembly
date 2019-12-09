# CRL.HeartcoreWebAssembly

This repo has sample code for a Blazor WebAssembly application integrating with Umbraco Heartcore.

## The Umbraco Setup
I have set up 2 doc types:

* AdventCalendar
* AdventCalendarDay - child nodes of AdventCalendar doctype, will represent each day.
** Day -  Numeric field representing the day in December
** Message - The message spoken by Alexa when this door is opened.

![](images/umbracoContent.PNG)

## Project Setup

This project is based on the Blazor project that is provided when you "file-> new" Blazor project. Remember to select the Web Assembly option. More info on this example and more about Blazor [here](https://docs.microsoft.com/en-gb/aspnet/core/blazor/get-started?view=aspnetcore-3.1&tabs=visual-studio).

Note, as WebAssemly is in preview at time of publishing, you may need Visual Studio 2019 preview version to see this option.

Next, we need to install the nuget package: Umbraco.Headless.Client.Net

## The code

Update the Index.razor use the Heartcore services and get the content. My example gets the nodes of type "AdventCalendarDate" and shows the Node name and message field for each day.

```
@page "/"

@using Umbraco.Headless.Client.Net.Delivery;

@if (content.Count > 0)
{
    foreach (var contentItem in content)
    {
        <p>@contentItem.Name</p>

        <p>@contentItem.Properties.Where(x=>x.Key=="message").FirstOrDefault().Value</p>
    }
}



@code {
    List<Umbraco.Headless.Client.Net.Delivery.Models.Content> content = new List<Umbraco.Headless.Client.Net.Delivery.Models.Content>();
    protected override async Task OnInitializedAsync()
    {


        var projectAlias = "caroles-inventive-kangaroo";
        var service = new ContentDeliveryService(projectAlias);
        var rootContentItems = await service.Content.GetByType("AdventCalendarDate");
        content = rootContentItems.Content.Items.ToList();
    }
}
```

## Test the application

![](images/blazorContent.PNG)
