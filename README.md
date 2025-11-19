# 18650 Battery Test Scripts

Professional battery testing scripts for 18650 lithium-ion cells using TestController and electronic loads (DL24, PX100, etc.).

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![TestController](https://img.shields.io/badge/Platform-TestController-green.svg)](https://lygte-info.dk/project/TestControllerIntro%20UK.html)

📖 **[Complete Testing Guide](TESTING_GUIDE.md)** - Comprehensive guide for testing and qualifying 18650 cells for safe reuse
🇩🇪 **[Deutsche Version](README_DE.md)** - German documentation

---

## Overview

This repository contains three professional battery testing scripts for characterizing 18650 lithium-ion cells:

1. **Basic Test** (`18650_test_script.txt`) - Detailed discharge test with periodic internal resistance monitoring
2. **Advanced Test** (`18650_advanced_test.txt`) - Multi-phase performance test with dynamic load ramping
3. **Pukker's Original** (`Battery Test with IntResistance Check AutosaveTussen.txt`) - Reference implementation with autosave features

### Key Features

- ✅ **2-Point Internal Resistance Measurement** (0.2C and 1C) for accurate IR testing
- ✅ **Comprehensive Safety Monitoring** (voltage, temperature, time limits)
- ✅ **Real-time Data Logging** with CSV export
- ✅ **Energy & Capacity Tracking** with health assessment
- ✅ **C-Rate Validation** to prevent battery damage
- ✅ **Periodic IR Measurements** (Basic test only) - monitor degradation during discharge
- ✅ **Dynamic Load Testing** (Advanced test only) - stress testing for high-drain applications

---

## Credits and Attribution

These scripts are based on and inspired by **Pukker's** excellent work and contributions from the TestController community.

**Original Source:** [Pukker's TestController User Scripts](https://lygte-info.dk/project/TestControllerUserScripts1%20UK.html#Battery_test_with_DL24/PX100_loads_by_Pukker)

### Pukker's Original Script (Included)

**Battery Test with IntResistance Check AutosaveTussen.txt** - Pukker's original script from the TestController community is included in this repository. It features:

- Internal Resistance Measurement (0.2C and 1C tests)
- Autosave functionality for charts, CSV, and logs
- Comprehensive safety monitoring (temperature, voltage, time)
- Flexible export options
- Math expressions for Power, Capacity, Energy, and Load Resistance

**Source:** [Pukker's Battery Test Scripts](https://lygte-info.dk/project/TestControllerUserScripts1%20UK.html#Battery_test_with_DL24/PX100_loads_by_Pukker)

---

## Test Scripts

### 1. Basic Test Script (`18650_test_script.txt`)

**Best for:** Standard capacity testing with degradation monitoring

A comprehensive constant-current discharge test with:
- Constant discharge current (default 1A)
- 2-point internal resistance measurement (0.2C and 1C) at start
- **Periodic IR measurements every 5 minutes** during discharge
- Voltage monitoring with drop rate detection
- Temperature monitoring (15°C - 50°C)
- Capacity calculation (mAh) and energy tracking (Wh)
- 1-second logging intervals
- Battery health assessment

**Typical Duration:** 2-4 hours

**Use Cases:**
- Standard capacity verification
- Cell screening for reuse projects
- Degradation monitoring over discharge cycle
- Quality control testing

---

### 2. Advanced Test Script (`18650_advanced_test.txt`)

**Best for:** Performance characterization under dynamic loads

A multi-phase test simulating real-world dynamic load conditions:

#### Test Phases:

1. **Conditioning Phase (15 min)**
   - Constant current at 1A
   - Battery stabilization

2. **Ramp Up Phase (15 min)**
   - Linear increase from 1A to 2A
   - 1-second measurement intervals

3. **Maximum Load Hold (15 min)**
   - Constant current at 2A
   - Stability testing under full load
   - Periodic IR measurements

4. **Ramp Down Phase (15 min)**
   - Linear decrease from 2A to 1A
   - Controlled load reduction

5. **Final Discharge**
   - Constant current at 1A
   - Until minimum voltage reached

**Typical Duration:** 6-8 hours

**Use Cases:**
- High-drain application testing (e-bikes, power tools)
- Cell performance characterization
- Stress testing for demanding applications
- Dynamic load response analysis

---

## Safety Features (All Scripts)

- ✅ Voltage monitoring (2.5V - 4.2V range)
- ✅ Temperature limits (15°C - 50°C configurable)
- ✅ C-rate validation with warnings
- ✅ Pre-test battery condition checks
- ✅ Automatic load shutdown on limit violations
- ✅ Time-based cutoffs
- ✅ Voltage drop rate monitoring (Basic test)

---

## Getting Started

### Requirements

- **TestController** software ([Download](https://lygte-info.dk/project/TestControllerIntro%20UK.html))
- **Electronic Load** compatible with TestController (DL24, PX100, etc.)
- **18650 cells** fully charged to 4.2V
- **Safety equipment** (LiPo bag, fire extinguisher, smoke detector)

### Installation

1. Clone this repository or download the script files
2. Copy scripts to your TestController script library:
   ```
   Documents\TestController\ScriptLibrary\
   ```
3. Scripts will appear in TestController's script menu

### Quick Start

1. **Fully charge** your 18650 cell to 4.2V
2. **Connect** cell to electronic load
3. **Open TestController** and select desired script
4. **Configure parameters** in popup dialog:
   - Nominal capacity (mAh)
   - Discharge current (A)
   - Safety limits (voltage, temperature)
5. **Start test** and monitor progress
6. **Review results** in CSV log files

**⚠️ Never leave tests unattended! Always monitor battery temperature and voltage.**

---

## Which Test Should I Use?

| Application | Recommended Script | Duration | Key Benefits |
|-------------|-------------------|----------|--------------|
| Quick capacity check | Pukker's Script | 2-4h | Fast, autosave, accurate IR |
| Standard testing | Basic Test | 2-4h | Degradation monitoring, detailed |
| High-drain apps | Advanced Test | 6-8h | Dynamic load, stress testing |
| Cell matching for packs | Basic Test | 2-4h | Periodic IR, capacity matching |

**📖 See [TESTING_GUIDE.md](TESTING_GUIDE.md) for detailed testing strategies and workflows**

---

## Output Data

All tests generate CSV files with:

- Time-stamped measurements
- Voltage (V), Current (A), Temperature (°C)
- Calculated capacity (mAh) and energy (Wh)
- Power measurements (W)
- Internal resistance measurements (mΩ)
- Phase indicators (Advanced test only)
- Real-time charting capabilities

---

## Configuration Examples

### Basic Test - Standard Configuration
```
Nominal Capacity:    3000 mAh
Discharge Current:   1.0 A (0.33C)
Minimum Voltage:     2.5 V
Start Voltage:       3.7 V
Maximum Test Time:   180 minutes
Max Temperature:     50°C
Min Temperature:     15°C
```

### Advanced Test - High-Drain Configuration
```
Nominal Capacity:       3000 mAh
Initial Current:        1.0 A
Ramp Start Current:     1.0 A
Ramp Maximum Current:   3.0 A (1C)
Ramp Duration:          15 minutes
Minimum Voltage:        2.5 V
Maximum Temperature:    50°C
```

---

## Technical Details

### Internal Resistance Measurement

Both custom scripts use Pukker's advanced 2-point method:

**Method:**
1. Apply 0.2C load for 10 seconds → Measure V₁ and I₁
2. Ramp to 1C load for 1 second → Measure V₂ and I₂
3. Calculate: `R = (V₁ - V₂) / (I₂ - I₁)`

**Advantages:**
- Larger current difference = higher accuracy
- Less susceptible to measurement noise
- Better resolution for low-resistance cells (< 50 mΩ)
- Stabilization period ensures accurate baseline

**Health Assessment:**
- < 50 mΩ: Excellent (suitable for high-drain)
- 50-70 mΩ: Good (suitable for medium-drain)
- 70-100 mΩ: Fair (low-drain only)
- \> 100 mΩ: Poor (discard)

### Energy Calculations

```
Capacity (mAh) = ∫ I(t) dt
Energy (Wh)    = ∫ V(t) × I(t) dt
Average V      = Energy / Capacity
```

---

## File Structure

```
.
├── 18650_test_script.txt          # Basic test (periodic IR monitoring)
├── 18650_advanced_test.txt        # Advanced multi-phase test
├── Battery Test with...txt        # Pukker's original script
├── README.md                      # This file (English)
├── README_DE.md                   # German documentation
├── TESTING_GUIDE.md               # Comprehensive testing guide
├── CLAUDE.md                      # Development documentation
└── LICENSE                        # Project license
```

---

## Documentation

- **[TESTING_GUIDE.md](TESTING_GUIDE.md)** - Complete guide for testing and qualifying cells for reuse
- **[CLAUDE.md](CLAUDE.md)** - Development documentation and TestController scripting reference
- **[README_DE.md](README_DE.md)** - German version of this README

---

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

### Areas for Contribution

- Additional test scripts for specific use cases
- Translations to other languages
- Bug fixes and improvements
- Documentation enhancements

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- **Pukker** - Original battery test scripts and inspiration
- **HKJ (Henrik)** - TestController software and documentation
- **TestController Community** - Valuable feedback and script contributions

---

## Safety Disclaimer

⚠️ **WARNING:** Testing lithium-ion batteries involves risks including fire and explosion.

- Always test in a safe environment with proper fire safety equipment
- Never leave tests unattended
- Use LiPo safety bags during testing
- Immediately discontinue testing if cells show signs of damage, swelling, or overheating
- Dispose of damaged cells properly according to local regulations

**The authors are not responsible for any damage or injury resulting from the use of these scripts.**

---

## Links

- [TestController Official Site](https://lygte-info.dk/project/TestControllerIntro%20UK.html)
- [Pukker's User Scripts Collection](https://lygte-info.dk/project/TestControllerUserScripts1%20UK.html)
- [Battery University - 18650 Guide](https://batteryuniversity.com/)

---

**Made with ❤️ for the DIY battery community**
