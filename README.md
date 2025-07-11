# Smart Witness ESP32-CAM Complete Firmware

## Professional IoT Camera System with Advanced Features

This is a complete, production-ready firmware for ESP32-CAM devices that provides advanced surveillance and monitoring capabilities through Telegram integration and BLE configuration.

## 🚀 Key Features

### **BLE Configuration System**
- Initial device setup via Bluetooth Low Energy
- Web-based configuration interface
- Secure PIN-based authentication
- No need for hardcoded WiFi credentials

### **Multi-Network WiFi Support**
- Up to 3 WiFi networks with automatic fallback
- Robust connection management
- Automatic reconnection on network failures

### **Advanced Telegram Integration**
- Multi-chat support (Personal/Family/Neighborhood)
- Role-based access control
- Inline keyboard menus
- Real-time photo capture and sharing
- Conversational configuration editing

### **Professional Camera System**
- High-quality photo capture
- Automatic flash control
- PSRAM optimization for better performance
- Multiple resolution support

### **Production-Grade Architecture**
- Robust state machine implementation
- Watchdog timer protection
- Memory leak prevention
- Comprehensive error handling
- Statistical monitoring

## 📋 System Requirements

### **Hardware**
- ESP32-CAM (AI Thinker model)
- MicroSD card (optional, for extended storage)
- 5V power supply (minimum 2A recommended)

### **Software Dependencies**
- Arduino IDE or PlatformIO
- ESP32 Arduino Core (v2.0.0 or later)
- Required Libraries:
  - WiFi
  - WiFiClientSecure
  - esp_camera
  - SPIFFS
  - BLEDevice
  - BLEServer
  - BLEUtils
  - BLE2902
  - ArduinoJson (v6.x)
  - esp_task_wdt
  - UniversalTelegramBot

## 🔧 Installation Guide

### **1. Hardware Setup**
```
ESP32-CAM Pin Configuration:
- GPIO 33: Status LED
- GPIO 4:  Flash LED
- Camera pins: Defined in camera_pins.h
```

### **2. Software Installation**

1. **Install Arduino IDE** (if not already installed)
   - Download from [arduino.cc](https://www.arduino.cc/en/software)

2. **Add ESP32 Board Package**
   - File → Preferences → Additional Board Manager URLs
   - Add: `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
   - Tools → Board → Boards Manager → Search "ESP32" → Install

3. **Install Required Libraries**
   ```
   Sketch → Include Library → Manage Libraries
   
   Install the following libraries:
   - ArduinoJson by Benoit Blanchon (v6.x)
   - UniversalTelegramBot by Brian Lough
   ```

4. **Upload Firmware**
   - Open `smart_witness_complete.ino`
   - Select Board: "AI Thinker ESP32-CAM"
   - Select Port: Your ESP32-CAM port
   - Upload the firmware

### **3. Initial Configuration**

1. **Power on the device**
   - Device will start in BLE configuration mode
   - LED will indicate status

2. **Configure via Web Interface**
   - Visit: [Smart Witness Configuration](https://arielzubigaray.github.io/smart-witness-config/)
   - Follow the setup wizard
   - Provide WiFi credentials and Telegram bot token

3. **Telegram Bot Setup**
   - Create a bot via [@BotFather](https://t.me/BotFather)
   - Get your bot token
   - Start a chat with your bot to get your chat ID

## 🔧 Configuration Parameters

### **Device Configuration**
- **Device Name**: Technical identifier (auto-generated)
- **Friendly Name**: Human-readable device name
- **Device PIN**: 3-digit security PIN

### **WiFi Networks (up to 3)**
- **SSID**: Network name
- **Password**: Network password
- **Priority**: Primary, secondary, tertiary

### **Telegram Configuration**
- **Bot Token**: Telegram bot token from BotFather
- **Personal Chat ID**: Your personal chat with the bot
- **Family Chat ID**: Optional family group chat
- **Neighborhood Chat ID**: Optional neighborhood group chat

### **Alert Configuration**
- **Alert Message**: Custom startup/alert message

## 📱 Telegram Commands

### **Universal Commands (All Chat Types)**
- `/photo` - Take and send a photo
- `/status` - Show device status and statistics

### **Personal Chat Only**
- `/menu` - Show main menu with all options
- `/config` - Access configuration menu
- `/restart` - Restart the device
- **Configuration editing**: Interactive setup for all parameters

### **Family/Neighborhood Chats**
- Limited to photo capture and basic status
- No configuration access for security

## 🏗️ System Architecture

### **State Machine**
```
NEED_CONFIG → BLE_CONFIG_PHASE → PIN_CONFIG_PHASE → NORMAL_OPERATION
     ↑                                                       ↓
     └─────────────────── Configuration Error ←──────────────┘
```

### **Phased Implementation (25 Functions)**

**Phase 1: Basic Configuration (Critical)**
- `loadDefaultConfig()` - Load default settings
- `loadConfig()` - Load from SPIFFS
- `saveConfig()` - Save to SPIFFS
- `hasValidConfig()` - Validate configuration
- `generateDevicePIN()` - Generate security PIN
- `generateDefaultFriendlyName()` - Create device name
- `showStoredConfig()` - Debug configuration

**Phase 2: BLE System (High Priority)**
- `initBLE()` - Initialize BLE server
- `updateBLEStatus()` - Status notifications
- `processBLEConfiguration()` - Process web config
- `triggerConfigurationRestart()` - Clean restart

**Phase 3: Camera System (High Priority)**
- `initCamera()` - Initialize camera hardware

**Phase 4: Telegram Commands (High Priority)**
- `sendStatus()` - System status
- `sendMainMenu()` - Main menu
- `sendConfigMenu()` - Configuration menu

**Phase 5: Keyboard Generation (High Priority)**
- `getCancelKeyboard()` - Cancel button
- `getMainMenuKeyboard()` - Main menu buttons
- `getConfigKeyboard()` - Configuration buttons

**Phase 6: Editing System (Medium-High Priority)**
- `startEditMode()` - Begin editing
- `showEditConfirmation()` - Confirm changes
- `confirmEdit()` - Apply changes
- `cancelEdit()` - Cancel editing

**Phase 7: Photo System (Medium-High Priority)**
- `takePhoto()` - Capture and send photos

**Phase 8: Alerts (Medium Priority)**
- `sendStartupAlertsToAllChats()` - Startup notifications

**Phase 9: Maintenance (Low Priority)**
- `handleSerialCommands()` - Debug commands

## 🔐 Security Features

### **Multi-Layer Security**
- BLE PIN authentication
- Chat type validation
- Command device name verification
- Session timeouts
- Rate limiting

### **Access Control**
- **Personal Chat**: Full control (configuration, restart, etc.)
- **Family Chat**: Limited access (photos, status)
- **Neighborhood Chat**: Alert-only access

## 📊 Monitoring & Statistics

### **System Health Monitoring**
- Memory usage tracking
- WiFi connection status
- Telegram polling statistics
- Message success/failure rates

### **Debug Features**
- Serial command interface
- Comprehensive logging
- Configuration inspection
- Network diagnostics

## 🚀 Advanced Features

### **Robust Error Handling**
- Automatic recovery from network failures
- Watchdog timer protection
- Memory leak prevention
- State machine error handling

### **Production Optimizations**
- First-boot protection
- Rate limiting
- Resource cleanup
- Efficient polling

## 🐛 Troubleshooting

### **Common Issues**

**Device not connecting to WiFi:**
- Check SSID/password in configuration
- Verify network is 2.4GHz (ESP32 doesn't support 5GHz)
- Check signal strength

**BLE configuration not working:**
- Ensure device is in configuration mode (no valid config)
- Check web interface compatibility
- Verify BLE is enabled on your device

**Telegram commands not responding:**
- Verify bot token is correct
- Check chat ID configuration
- Ensure device has internet connectivity

**Camera not working:**
- Check camera module connection
- Verify sufficient power supply (2A minimum)
- Check for hardware conflicts

### **Serial Commands for Debugging**
- `status` - Show full system status
- `restart` - Restart device
- `reset` - Reset configuration
- `test_photo` - Take test photo
- `wifi_scan` - Scan available networks
- `send <message>` - Send test message

## 📈 Performance Specifications

### **Memory Usage**
- Minimum free heap: 50KB (warning threshold)
- Configuration storage: <2KB JSON
- Image buffer: Variable (PSRAM dependent)

### **Network Performance**
- Telegram polling: 2-second intervals
- Message rate limiting: 1.5-second intervals
- WiFi reconnection: 15-second timeout per network

### **Power Consumption**
- Idle: ~120mA @ 5V
- WiFi active: ~200mA @ 5V
- Camera capture: ~300mA peak @ 5V

## 🛠️ Development Guide

### **Code Structure**
```
smart_witness_complete.ino  # Main firmware file
camera_pins.h              # Camera pin definitions
README.md                  # This documentation
```

### **Adding New Features**
1. Follow the phased implementation approach
2. Maintain state machine architecture
3. Add proper error handling
4. Update documentation

### **Testing Procedures**
1. Unit test individual functions
2. Integration test with real hardware
3. Field test with multiple network conditions
4. Stress test with continuous operation

## 📄 License

This project is open source and available under the MIT License.

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Add comprehensive tests
4. Submit a pull request

## 📞 Support

For technical support:
- Create an issue in the GitHub repository
- Provide detailed error logs
- Include hardware configuration details

## 🔄 Version History

### v2.1.0-Production (Current)
- Complete implementation of all 25 functions
- Production-grade error handling
- Multi-chat Telegram support
- BLE configuration system
- Robust state machine architecture

---

**Smart Witness ESP32-CAM** - Professional IoT camera surveillance system with advanced Telegram integration and BLE configuration capabilities.