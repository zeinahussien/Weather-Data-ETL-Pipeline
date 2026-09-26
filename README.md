# 🌤️ Weather Data ETL Pipeline

An end-to-end Extract, Transform, Load (ETL) data pipeline built in Python to collect real-time weather information from the OpenWeatherMap API, process and clean the data using Pandas, and securely load it into an Azure SQL Database.

---

## 🛠️ Tech Stack
* **Language:** Python (Jupyter Notebooks)
* **Data Manipulation:** Pandas, DateTime
* **API & HTTP Requests:** Requests
* **Database Connectivity:** PyODBC, Azure SQL Server
* **Environment Security:** Python-Dotenv (`.env`)
* **Version Control:** Git & GitHub

---

## 📂 Target Cities & Regions
The pipeline extracts weather data across 10 key cities in Egypt, categorizing them into distinct geographical regions:
* **Greater Cairo:** Cairo, Giza
* **North Coast:** Alexandria
* **Canal:** Port Said, Suez, Ismailia
* **Lower Egypt:** Mansoura, Tanta
* **Upper Egypt:** Luxor, Aswan

---

## 🔄 ETL Pipeline Architecture

### 1. Extract (`Extraction.ipynb`)
* Connects securely to the **OpenWeatherMap API** using an environment-managed API token.
* Iterates through the list of target cities, fetching current weather payloads in JSON format.
* Parses essential meteorological attributes (temperature, pressure, humidity, wind speed, coordinates, weather conditions).
* Exports raw extracted payloads to `raw_cities_data.csv`.

### 2. Transformation (`Transformation.ipynb`)
* Reads `raw_cities_data.csv` into a Pandas DataFrame.
* **Data Cleaning & Conversions:** Converts temperatures from Kelvin to Celsius and rounds coordinates/temperatures to 2 decimal places.
* **Categorization Logic:**
  * Maps cities to custom **`Region`** classifications.
  * Classifies temperatures into **`TemperatureCategory`** (*Cold*, *Moderate*, *Hot*).
  * Classifies humidity levels into **`HumidityCategory`** (*Low*, *Medium*, *High*).
* **Metadata Enhancement:** Appends an exact **`IngestionTime`** timestamp.
* Exports the cleaned dataset to `transformed_cities_data.csv`.

### 3. Loading (`Loading.ipynb`)
* Pulls secure database credentials (`DB_SERVER`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`) dynamically from a local `.env` file.
* Establishes a secure connection to **Azure SQL Server** via `pyodbc`.
* Programmatically manages schema creation (`transformed_data` table).
* Performs efficient bulk insertion using parameterized queries (`cursor.executemany()`) to safeguard against SQL injection.

---

## 📊 Database Schema (`transformed_data`)

| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `city` | VARCHAR(50) | Name of the city |
| `country` | VARCHAR(50) | Country code (e.g., EG) |
| `latitude` | VARCHAR(50) | Geographical latitude (rounded) |
| `longitude` | VARCHAR(50) | Geographical longitude (rounded) |
| `temperature` | VARCHAR(50) | Temperature in Celsius (°C) |
| `pressure` | VARCHAR(50) | Atmospheric pressure |
| `humidity` | VARCHAR(50) | Humidity percentage |
| `wind_speed` | VARCHAR(50) | Wind speed |
| `weather_main` | VARCHAR(50) | Primary weather condition |
| `weather_desc` | VARCHAR(50) | Detailed weather description |
| `region` | VARCHAR(50) | Egyptian region classification |
| `temperature_category` | VARCHAR(50) | Cold, Moderate, or Hot |
| `humidity_category` | VARCHAR(50) | Low, Medium, or High |
| `ingestion_time` | VARCHAR(50) | Timestamp of ETL execution |

---

## 🔒 Security Best Practices
* **Credentials Management:** API tokens and Azure SQL login credentials are completely decoupled from source code using a local `.env` file.
* **Git Protection:** Sensitive configuration and staging files (`.env`, `*.csv`) are strictly excluded from version control via `.gitignore`.

---

## 🚀 How to Run the Project
1. Clone the repository:
   ```bash
   git clone [https://github.com/your-username/Weather-Data-ETL-Pipeline.git](https://github.com/your-username/Weather-Data-ETL-Pipeline.git)
