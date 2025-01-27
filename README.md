# Weather-Application
import React, { useState } from "react";
import axios from "axios";

const App = () => {
  const [city, setCity] = useState("");
  const [weather, setWeather] = useState(null);
  const [error, setError] = useState("");

  const apiKey = "YOUR_API_KEY"; // Replace with your OpenWeatherMap API Key
  const fetchWeather = async () => {
    if (!city) {
      setError("Please enter a city.");
      return;
    }

    try {
      const response = await axios.get(
        `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`
      );
      setWeather(response.data);
      setError(""); // Clear any previous errors
    } catch (err) {
      setError("City not found. Please try again.");
      setWeather(null);
    }
  };

  return (
    <div className="app" style={styles.container}>
      <h1 style={styles.title}>Weather App</h1>
      <input
        type="text"
        placeholder="Enter city name"
        value={city}
        onChange={(e) => setCity(e.target.value)}
        style={styles.input}
      />
      <button onClick={fetchWeather} style={styles.button}>
        Get Weather
      </button>
      {error && <p style={styles.error}>{error}</p>}
      {weather && (
        <div style={styles.weatherCard}>
          <h2>{weather.name}</h2>
          <p>Temperature: {weather.main.temp}°C</p>
          <p>Condition: {weather.weather[0].description}</p>
          <p>Humidity: {weather.main.humidity}%</p>
          <p>Wind Speed: {weather.wind.speed} m/s</p>
        </div>
      )}
    </div>
  );
};

// Basic inline styles
const styles = {
  container: {
    textAlign: "center",
    fontFamily: "Arial, sans-serif",
    marginTop: "50px",
  },
  title: {
    fontSize: "2.5rem",
    marginBottom: "20px",
  },
  input: {
    padding: "10px",
    width: "200px",
    fontSize: "1rem",
  },
  button: {
    padding: "10px 15px",
    marginLeft: "10px",
    fontSize: "1rem",
    cursor: "pointer",
    backgroundColor: "#007BFF",
    color: "white",
    border: "none",
    borderRadius: "5px",
  },
  error: {
    color: "red",
    marginTop: "20px",
  },
  weatherCard: {
    marginTop: "30px",
    padding: "20px",
    border: "1px solid #ddd",
    borderRadius: "10px",
    display: "inline-block",
    textAlign: "left",
  },
};

export default App;
