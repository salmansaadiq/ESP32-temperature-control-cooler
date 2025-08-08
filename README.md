# ESP32 Temperature-Controlled Cooling Fan

An automatic cooling fan system that monitors ambient temperature using a DHT11 sensor and controls fan speed through an ESP32 microcontroller and L298N motor driver.

## 🌡️ Features

- **Real-time temperature monitoring** with DHT11 sensor
- **Variable fan speed control** based on temperature thresholds
- **WiFi connectivity** for remote access
- **Web server dashboard** for real-time monitoring and control
- **Remote fan control** via web interface
- **Automatic fan activation/deactivation** 
- **Serial monitor output** for temperature and fan status
- **Customizable temperature thresholds**
- **Energy efficient** - fan only runs when needed

## 📋 Components Required

| Component | Quantity | Purpose |
|-----------|----------|---------|
| ESP32 Development Board | 1 | Main microcontroller |
| DHT11 Temperature & Humidity Sensor | 1 | Temperature sensing |
| L298N Motor Driver Module | 1 | Fan motor control |
| DC Fan (12V recommended) | 1 | Cooling element |
| Jumper Wires | Various | Connections |
| Breadboard | 1 | Prototyping |
| 12V Power Supply | 1 | Power for fan |
| 10kΩ Resistor | 1 | Pull-up for DHT11 |

## 🔌 Circuit Diagram

```
ESP32          DHT11
-----          -----
3.3V    -----> VCC
GND     -----> GND
GPIO4   -----> DATA (with 10kΩ pull-up to VCC)

ESP32          L298N
-----          -----
GPIO16  -----> IN1
GPIO17  -----> IN2
GPIO18  -----> ENA (PWM)
GND     -----> GND
VIN     -----> +12V (from power supply)

L298N          DC Fan
-----          -------
OUT1    -----> Fan +
OUT2    -----> Fan -
```

## 🔧 Wiring Instructions

1. **DHT11 Sensor:**
   - VCC → ESP32 3.3V
   - GND → ESP32 GND
   - DATA → ESP32 GPIO4 (with 10kΩ pull-up resistor to 3.3V)

2. **L298N Motor Driver:**
   - IN1 → ESP32 GPIO16
   - IN2 → ESP32 GPIO17
   - ENA → ESP32 GPIO18 (PWM control)
   - GND → ESP32 GND
   - Connect 12V power supply to L298N power input

3. **DC Fan:**
   - Connect fan wires to OUT1 and OUT2 on L298N

## 📦 Required Libraries

Install these libraries through Arduino IDE Library Manager:

```cpp
#include <WiFi.h>           // ESP32 WiFi library (built-in)
#include <WebServer.h>      // ESP32 WebServer library (built-in) 
#include <DHT.h>            // DHT sensor library by Adafruit
```

Or install via Arduino IDE:
- Tools → Manage Libraries → Search "DHT sensor library"
- WiFi and WebServer libraries are included with ESP32 core

## ⚙️ Configuration

### WiFi Settings
Add your network credentials:
```cpp
const char* ssid = "YOUR_WIFI_NETWORK";
const char* password = "YOUR_WIFI_PASSWORD";
```

### Temperature Thresholds
Modify these parameters to suit your needs:
```cpp
#define TEMP_THRESHOLD_LOW  25    // Temperature (°C) to start fan at low speed
#define TEMP_THRESHOLD_MID  30    // Temperature (°C) to run fan at medium speed  
#define TEMP_THRESHOLD_HIGH 35    // Temperature (°C) to run fan at maximum speed
#define TEMP_HYSTERESIS     2     // Temperature difference to prevent rapid on/off
```

### Web Server Settings
```cpp
#define WEB_SERVER_PORT 80        // Default HTTP port
```

## 🚀 How It Works

1. **WiFi Connection:** ESP32 connects to your WiFi network on startup
2. **Web Server Initialization:** Creates a web server accessible via the ESP32's IP address
3. **Temperature Monitoring:** DHT11 sensor continuously reads ambient temperature
4. **Web Dashboard:** Real-time display of temperature, humidity, and fan status
5. **Remote Control:** Web interface allows manual fan override and settings adjustment
6. **Automatic Mode:** System can run in automatic mode based on temperature thresholds:
   - Below 25°C: Fan off
   - 25-30°C: Low speed (40% PWM)
   - 30-35°C: Medium speed (70% PWM)
   - Above 35°C: Maximum speed (100% PWM)
7. **Hysteresis:** Prevents rapid fan switching by requiring temperature to drop 2°C below threshold

## 🌐 Web Interface Features

- **Real-time temperature and humidity display**
- **Current fan speed indicator**
- **Manual fan control buttons** (Off, Low, Medium, High)
- **Automatic/Manual mode toggle**
- **Temperature threshold adjustment**
- **System status indicators**
- **Responsive design** for mobile and desktop

## 📊 Serial Monitor Output

```
WiFi connecting...
WiFi connected! IP address: 192.168.1.100
Web server started on port 80
Temperature: 28.5°C | Humidity: 65% | Fan Speed: Medium (70%) | Mode: Auto
Temperature: 31.2°C | Humidity: 62% | Fan Speed: High (100%) | Mode: Auto
Web request: Manual override - Fan set to Low
Temperature: 24.8°C | Humidity: 68% | Fan Speed: Low (Manual) | Mode: Manual
```

## 🌐 Accessing the Web Dashboard

1. **Find ESP32 IP Address:** Check serial monitor for IP address after WiFi connection
2. **Open Web Browser:** Navigate to `http://[ESP32_IP_ADDRESS]` (e.g., `http://192.168.1.100`)
3. **Control Your Fan:** Use the web interface to monitor and control your cooling system

## 🛠️ Installation & Setup

1. **Clone this repository:**
   ```bash
   git clone https://github.com/yourusername/esp32-cooling-fan.git
   ```

2. **Open in Arduino IDE:**
   - Install ESP32 board package
   - Install required libraries
   - Open `cooling_fan.ino`

3. **Configure settings:**
   - Set your WiFi credentials in the code
   - Adjust temperature thresholds as needed
   - Verify pin assignments match your wiring
   - Note down the ESP32's IP address from serial monitor

4. **Upload to ESP32:**
   - Select correct board: "ESP32 Dev Module"
   - Choose appropriate COM port
   - Upload the sketch

## 🔍 Troubleshooting

**WiFi not connecting:**
- Double-check SSID and password in code
- Ensure WiFi network is 2.4GHz (ESP32 doesn't support 5GHz)
- Check WiFi signal strength at ESP32 location

**Can't access web dashboard:**
- Verify ESP32 IP address from serial monitor
- Ensure ESP32 and device are on same network
- Try disabling firewall temporarily
- Check if port 80 is blocked

**Fan not starting:**
- Check 12V power supply connection
- Verify L298N wiring
- Ensure ENA jumper is connected or PWM signal is high enough

**Temperature readings seem wrong:**
- Verify DHT11 wiring and pull-up resistor
- Check for loose connections
- DHT11 has ±2°C accuracy - this is normal

**Web interface not updating:**
- Refresh the browser page
- Check browser console for JavaScript errors
- Ensure stable WiFi connection

**Erratic fan behavior:**
- Check for proper grounding between ESP32 and L298N
- Verify PWM frequency compatibility with your fan
- Ensure stable power supply

## 🔄 Future Enhancements

- [ ] Mobile app for iOS/Android
- [ ] Multiple temperature sensor support
- [ ] Data logging and historical graphs
- [ ] MQTT integration for IoT platforms
- [ ] Email/SMS alerts for high temperatures
- [ ] Automatic scheduling modes (timer-based control)
- [ ] OTA (Over-The-Air) firmware updates
- [ ] Integration with home automation systems
- [ ] API endpoints for third-party integration

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Adafruit for the excellent DHT library
- ESP32 community for helpful resources
- Arduino IDE team for the development environment
