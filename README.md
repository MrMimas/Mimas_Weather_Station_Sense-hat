# Not worked on Currently Feel free to try to fix it some.

# 🌤️ Mimas Weather Station

A weather prediction system using Raspberry Pi with Sense HAT, optimized for Swedish climate patterns and indoor placement.

[![Swedish Rain data used](https://img.shields.io/badge/🇸🇪25years-Swedish%20Rain%20Data-blue)](https://www.smhi.se/)
[![Raspberrypi Project](https://img.shields.io/badge/Raspberry_Pi-Project-red)](https://www.raspberrypi.org/)   
[![Sense HAT](https://img.shields.io/badge/Sense_HAT-Official-green)](https://www.raspberrypi.com/products/sense-hat/)
[![Local Operation](https://img.shields.io/badge/🏠-Local%20Operation-brightgreen)](#local-operation-no-internet-required)

## ✨ Features

### 🏠 **Indoor Placement Optimized** (Current Implementation)
- 🔥 **Adaptive temperature compensation** for hot Pi conditions
- 🌡️ **CPU temperature monitoring** with dynamic offset adjustment  
- 💧 **Indoor humidity correction** to estimate outdoor equivalents
- 📏 **Raw sensor data tracking** for calibration purposes
- 🪟 **Window placement optimization** for best indoor→outdoor predictions

> ⚠️ **Note**: The current project is designed for indoor use.

### 🇸🇪 **Swedish Climate Intelligence** 
- 📊 **25 years of SMHI climate data** integration (2000-2024)
- ❄️ **Winter-aware predictions** (snow vs rain detection)
- 🌦️ **Seasonal adjustment factors** for accurate forecasting
- 📈 **Historical pattern analysis** from your own data

### 💾 **Local Data Storage** 
- 📄 **CSV export** with all sensor data (Excel-compatible)
- 🗂️ **JSON logs** with detailed analysis
- 📈 **Historical trends** for pattern recognition  
- 🔍 **Diagnostic information** for troubleshooting
- 🏠 **100% offline operation** - no server required!

### 🌐 **Optional Remote Features**
- ☁️ **Real-time server uploads** with retry logic
- 📱 **Beautiful web dashboard** with live updates
- 🔗 **REST API endpoints** for integration
- 📊 **24-hour history tracking**

## 🛠️ Hardware Requirements

### Required
- 🥧 **Raspberry Pi 4B+**
- 🌡️ **Official Sense HAT** from Raspberry Pi Foundation
- 💿 **MicroSD Card** (32GB+ recommended)
- 🔌 **Power Supply** (official Pi power adapter)

### Recommended
- **Case with sensehat support** 
- **Open window** (for indoor placement near window)

### For Outdoor Use (Future Enhancement or you could do it before me)
- **Weatherproof enclosure**
- **Solar panel + battery system** for remote power
- **Rain gauge sensor** for precipitation verification
- **Wind speed/direction sensors** 
- **UV protection** for display and sensors


## Setup

### Option 1: Local Setup (Standalone - No Server Required) ✨ **Recommended for Beginners**

Perfect for running the weather station completely offline with local file logging:

```bash
# Clone the repository
git clone https://github.com/MrMimas/Mimas_Weather_Station_Sense-hat
cd Mimas_Weather_Station_Sense-hat

# Install dependencies
pip install -r requirements.txt

# Copy and configure settings for local use
cp config.example.json config.json
nano config.json  # Set "server": {"enabled": false}

# Run the weather station locally
python Raspi-Rain-predicter_Public.py
```

**Local Features:**
- 📄 **CSV logging** to `Mimas-Rain-Predicter.csv`
- 🗂️ **JSON logs** to `swedish_weather_log.json`
- 🌈 **LED status display** on Sense HAT
- 📊 **Console output** with detailed predictions
- ❄️ **Works offline**

### Option 2: Full Setup with Web Server (Advanced)

For remote monitoring and web dashboard:

```bash
# Same as local setup, but enable server in config.json
nano config.json  # Set "server": {"enabled": true, "server_url": "http://your-server."}

# Run the weather station (will upload to server)
python Raspi-Rain-predicter_Public.py

# Separately run the web server (on same Pi or different machine)
python weather_server_Public.py
```

### 3. Auto-start on Boot (Optional)
```bash
# Add to crontab for automatic startup
crontab -e
# Add: @reboot cd /path/to/mimas-weather-station && python Raspi-Rain-predicter_Public.py
```

## ⚙️ Configuration

#### Local Setup Config (`configPublic.json`)

### Weather Station Config (`config.json`)

```json
{
  "weather_station": {
    "temp_offset": -8,              // Base temperature compensation
    "adaptive_temp_compensation": true,  // Enable CPU-based adjustment
    "max_temp_offset": -12,         // Maximum compensation for hot Pi
    "cpu_temp_threshold": 60,       // CPU temp to start extra compensation
    "reading_interval": 300,        // Seconds between readings (5 min)
    "indoor_placement": true,       // Optimized for indoor use
    "csv_log_file": "Mimas-Rain-Predicter.csv",     // Local CSV file
    "json_log_file": "swedish_weather_log.json"     // Local JSON file
  },
  "server": {
    "enabled": false,              // ✅ DISABLED for local-only operation
    "server_url": "",              // Not needed for local setup
    "api_key": "",                 // Not needed for local setup
    "station_id": "my_weather_station"  // Just for local identification
  }
}
```

#### Full Setup Configuration (With Server)
```json
{
  "weather_station": {
    // ... same as above ...
  },
  "server": {
    "enabled": true,               // ✅ ENABLED for server uploads
    "server_url": "http://your-server.com",      // Replace with your actual server URL
    "api_key": "your_secure_api_key",            // Your secure API key
    "station_id": "your_station_name"            // Unique station identifier
  }
}
```

### Temperature Compensation Examples
- **Normal Pi (50°C CPU)**: Uses base `-8°C` offset
- **Warm Pi (65°C CPU)**: Uses `-8.5°C` offset (extra -0.5°C)
- **Hot Pi (80°C CPU)**: Uses `-10°C` offset (extra -2°C)
- **Very Hot Pi (90°C+)**: Uses max `-12°C` offset

### Console Display
```
🇸🇪 Swedish Indoor Weather Station - 2024-10-24 15:30:00
🪟 Located: Inside by window
============================================================
🌡️  Temperature: 21.5°C (indoor) / 18.5°C (est. outdoor)
🔥 Pi CPU Temperature: 65.2°C (compensation: -8.5°C)
📏 Raw Sense HAT temp: 30.0°C → Compensated: 21.5°C
💧 Humidity: 45.2% (indoor) / 55.2% (est. outdoor)
📊 Pressure: 1015.3 hPa (same indoors/outdoors)

🌧️  ☔ Rain likely - expect within 6 hours
🎯 Confidence: 65%
🗓️  Seasonal factor: 1.1x

📊 Swedish Climate Context (2024):
   Expected yearly rainfall: 712 mm
   Long-term average: 698.5 mm
   ☔ Wet year (+13 mm above average)

🔍 Contributing factors:
   • High humidity (55.2%, threshold: 60.5%)
   • Pressure falling (-1.8 hPa)
   • Peak wet season (factor: 1.1)
```

### LED Status Indicators
- 🔵 **Blue**: Taking sensor reading
- 🟢 **Green**: Clear weather
- 🟡 **Yellow**: Cloudy conditions  
- 🟠 **Orange**: Rain likely
- 🔴 **Red**: Rain imminent
- ⚪ **White**: Snow conditions (winter)
- 🔷 **Light Blue**: Clear winter weather

## 🌐 Web Dashboard (Optional)

When running with server enabled, access your weather data through the beautiful web interface:

- **📊 Real-time readings** with adaptive Pi compensation
- **🎯 Confidence levels** for weather predictions
- **📈 24-hour trends** and historical data
- **🔗 API endpoints** for custom integrations
- **❄️ Winter mode** with snow/rain differentiation

### API Endpoints (Server Mode Only)
```bash
GET /api/health               # Server health check
GET /api/weather/latest       # Current weather data
GET /api/weather/history/24h  # 24-hour history
POST /api/weather/upload      # Upload data (Pi → Server)
```

## 📈 Swedish Climate Intelligence

### Historical Data Integration
- **📊 25 years** of Swedish Meteorological and Hydrological Institute (SMHI) data
- **🌧️ Rainfall patterns** from 2000-2024
- **❄️ Winter precipitation** vs summer rain detection
- **📅 Seasonal factors** for accurate monthly predictions

### Notable Swedish Weather Years
- **2000**: 828mm - Extremely wet year  
- **2018**: 530mm - Driest year in dataset
- **2023**: 786mm - Very wet year
- **2024**: 712mm - Above average

## 🏠 Local Operation (No Internet Required!)

The weather station works perfectly **without any server or internet connection**:

### What You Get Locally:
- ✅ **Full weather predictions** using 25 years of Swedish climate data
- ✅ **CSV data export** with all sensor readings and predictions
- ✅ **JSON logging** with detailed analysis and factors
- ✅ **LED status display** showing current weather conditions
- ✅ **Console output** with real-time predictions and diagnostics
- ✅ **Historical pattern analysis** from your accumulated data
- ✅ **Winter/summer seasonal adjustments** 
- ✅ **Adaptive Pi temperature compensation**

### Viewing Your Data:
- **📊 Open CSV in Excel/LibreOffice** for charts and analysis
- **📱 Use any text editor** to view JSON logs
- **📈 Import into data analysis tools** (Python, R, etc.)
- **🖥️ Monitor console output** for real-time predictions


## 🤝 Contributing 💡 Ideas Welcome

**I welcome contributions for outdoor enhancements!** Priority areas for improvement:

### 🌟 **High Priority:**
- 🌍 **Outdoor hardware integration** - Outdoor support and guide 
- 🔋 **Power management systems** - Solar/battery optimization
- 🌧️ **Physical rain detection** - Rain gauge integration  
- 💨 **Wind monitoring** - wind data support

### 🚀 **Medium Priority:**
- 🌍 **International climate data** for other countries
- 📱 **Mobile app** for remote monitoring  
- 🏠 **Multiple sensor support** for larger deployments
- 📊 **Advanced ML algorithms** for enhanced predictions



## 📝 License

MIT License - see [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments and Sources

- **🇸🇪 SMHI (Swedish Meteorological and Hydrological Institute)** for the wonderful climate data
- **🥧 Raspberry Pi Foundation** for excellent hardware and the Sense HAT


## 📞 Support

- **💬 Discussions**: [GitHub Discussions](https://github.com/Mimas_Weather_Station_Sense-hat/)
- **📧 Email**: tranlens@tranlens.se

---

**🌤️ Made with care in Sweden 🇸🇪**
