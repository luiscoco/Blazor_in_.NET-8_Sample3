# Blazor in .NET 8: Sample3

**Example: Weather Forecast App**

**Features demonstrated**:

**Routing**: Navigating between different views in your Blazor application

**Route Parameters**: Passing information through the URL

**Data Fetching**: Retrieving data from an external API

**Prerequisites**

An understanding of basic Blazor concepts (from previous examples)

A free API key from OpenWeatherMap to fetch weather data (https://openweathermap.org/appid)

Replace YOUR_API_KEY in the code below with your actual key

**Setup**

Create a new Blazor Server project (or reuse an existing one). Name it something like "MyWeatherApp"

**Models**

Create a Models folder. Add the following C# classes:

**WeatherForecast.cs**

```csharp
namespace MyWeatherApp.Models;

public class WeatherForecast
{
    public DateTime Date { get; set; }
    public int TemperatureC { get; set; }
    public string Summary { get; set; }
}
```

**WeatherData.cs**

```csharp
namespace MyWeatherApp.Models;

// You'll need to add more properties here to match the full API response. 
// Explore the OpenWeatherMap 'One Call API' documentation for details.
public class WeatherData 
{
    public List<WeatherForecast> Daily { get; set; } 
} 
```

**Components**

**FetchData.razor**: (Inside the Pages folder)

```csharp
@page "/fetchdata"
@inject HttpClient Http

<h1>Weather Forecast</h1>

@if (forecasts == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <table class="table">
        <thead>
            <tr>
                <th>Date</th>
                <th>Temp. (C)</th>
                <th>Summary</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var forecast in forecasts)
            {
                <tr>
                    <td>@forecast.Date.ToShortDateString()</td>
                    <td>@forecast.TemperatureC</td>
                    <td>@forecast.Summary</td>
                </tr>
            }
        </tbody>
    </table>
}

@code {
    private WeatherData forecasts;

    protected override async Task OnInitializedAsync()
    {
        forecasts = await Http.GetFromJsonAsync<WeatherData>("https://api.openweathermap.org/data/2.5/onecall?lat=40.4168&lon=-3.7038&exclude=current,minutely,hourly,alerts&units=metric&appid=YOUR_API_KEY");
    }
}
```

**WeatherForecast.razor (Inside the Pages folder)**:

```csharp
@page "/weather/{city}" 

<h1>Weather Forecast for @City</h1>

@inject HttpClient Http

@if (forecast == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <div class="card">
        <div class="card-body">
            <h5 class="card-title">@forecast.Date.ToShortDateString()</h5>
            <p class="card-text">@forecast.Summary (@forecast.TemperatureC &#8451;)</p>
        </div>
    </div>
}

@code {
    [Parameter] public string City { get; set; } 
    private WeatherForecast forecast;

    protected override async Task OnInitializedAsync()
    {
        // ... (code to fetch data based on 'City')
    }
}
```

