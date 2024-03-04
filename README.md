# Weather-Forecast-APP

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Weather Dashboard</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css">
<link rel="stylesheet" href="styles.css">
</head>
<body>
<div class="container mt-5">
  <h1 class="text-center">Weather Forecast</h1>
  <div class="row justify-content-center my-4">
      <div class="col-md-6">
          <!-- Input group for city and country -->
          <div class="input-group mb-3">
              <input type="text" id="city-input" class="form-control"
              placeholder="City" aria-label="City">
              <input type="text" id="country-input" class="form-control"
              placeholder="Country Code (e.g., UK)" aria-label="Country Code">
              <div class="input-group-append">
                  <button class="btn btn-outline-secondary" type="button"
                  id="search-btn">Get Forecast</button>
              </div>
          </div>
          <!-- Weather display area -->
          <div id="weather-result" class="text-center">
              <!-- Weather information will be displayed here -->
          </div>
      </div>
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
<script src="weather.js"></script>
</body>
</html>
CSS Source Code:

.weather-day {
  margin-bottom: 10px;
}
JavaScript Source Code:

const apiKey = 'YOUR_API_KEY'; // Replace with your API key
const weatherApiUrl = 'https://api.openweathermap.org/data/2.5/weather';

async function fetchWeather(city, country) {
  const queryParams = new URLSearchParams({
      q: `${city},${country}`,
      appid: apiKey,
      units: 'metric' // Or 'imperial' for Fahrenheit
  });

  try {
      const response = await fetch(`${weatherApiUrl}?${queryParams}`);
      if (!response.ok) throw new Error(`Weather data fetch failed: ${response.statusText}`);
      const weatherData = await response.json();
      updateWeatherUI(weatherData);
  } catch (error) {
      console.error(error);
      alert(error.message);
  }
}

function updateWeatherUI(weatherData) {
  const weatherResultDiv = document.getElementById('weather-result');
  // Clear previous results
  weatherResultDiv.innerHTML = '';
  // Bootstrap card for weather data
  const weatherCard = `
      <div class="card">
          <div class="card-body">
              <h5 class="card-title">${weatherData.name}</h5>
              <h6 class="card-subtitle mb-2 text-muted">${weatherData.weather[0].main}</h6>
              <p class="card-text">Temperature: ${weatherData.main.temp}°C</p>
              <p class="card-text">Feels like: ${weatherData.main.feels_like}°C</p>
              <p class="card-text">Wind Speed: ${weatherData.wind.speed} m/s</p>
              <p class="card-text">Humidity: ${weatherData.main.humidity}%</p>
          </div>
      </div>
  `;
  weatherResultDiv.innerHTML = weatherCard;
}

document.getElementById('search-btn').addEventListener('click', () => {
  const cityInput = document.getElementById('city-input').value.trim();
  const countryInput = document.getElementById('country-input').value.trim().toLowerCase();
  if (cityInput && countryInput) {
      fetchWeather(cityInput, countryInput);
  } else {
      alert('Please enter both city and country code.');
  }
});
