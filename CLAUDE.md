# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## TestController Platform Context

These scripts are designed for the TestController application, which supports user-created scripts for electronic test equipment. The TestController platform:
- Enables community script sharing and development
- Supports various electronic loads (DL24, PX100, etc.)
- Uses a custom scripting language for hardware control
- Scripts are stored in `Documents\TestController\ScriptLibrary` as "*Script.txt" files
- Scripts can be accessed via `#scriptMenu` popup menus in the application

This repository contains 18650 battery test scripts inspired by and built upon community contributions from the TestController user base, particularly work by developers like Pukker who created similar battery testing and power supply testing scripts.

## Project Overview

This repository contains battery testing scripts for 18650 lithium-ion batteries written in a custom test controller scripting language. The scripts control electronic load devices to perform discharge tests with comprehensive data logging and safety monitoring.

## Architecture

The codebase consists of two main test scripts:

1. **Basic Test Script** (`18650_test_script.txt`): Simple constant current discharge test
2. **Advanced Test Script** (`18650_advanced_test.txt`): Multi-phase test with ramping and conditioning

Both scripts follow the same architectural pattern:
- Device initialization and configuration via popup dialogs
- Safety checks (voltage, temperature, time limits)
- Load control with current/voltage settings
- Real-time data logging with CSV export
- Graceful shutdown and test completion reporting

## Script Language Features

The scripts use a custom test controller language with these key constructs:

### Control Flow
- `=var` for variable declarations
- `#if`, `#elif`, `#else`, `#endif`, `#while`, `#endwhile` for control flow
- `#return` for early script termination
- `break` for exiting loops
- `#wait` for conditional waiting patterns (e.g., `#wait (voltage > 3.0) and time < 1000`)

### User Interface
- `#popupInit`, `#popupNumber`, `#popupButtons`, `#popupCheckbox`, `#popupText` for configuration dialogs
- `#scriptMenu` for adding scripts to application menus

### Data Logging & Charting
- `#log`, `#logData`, `#logFile`, `#logStart`, `#logClear`, `#logTime` for data logging
- `#HasLogged` to ensure data point is logged before proceeding
- `#ChartCurves`, `#chartColors`, `#chartScaleName`, `#chartX` for real-time visualization
- `#Charttitle` for setting chart titles

### Mathematical Expressions (from Pukker's script)
- `#math` for calculated values (Power, Capacity, Energy, Resistance)
- Example: `#math Power "W" Formula 0 ((nameCurrent(load))+"*"+(nameVoltage(load)))`
- `#math` types: `Formula`, `SumTimeHour` for integration over time

### Export & Autosave (from Pukker's script)
- `#ExportInit`, `#ExportColumns`, `#ExportColumn`, `#ExportReduce`, `#ExportTable` for CSV export
- `#Savechart`, `#Savelog`, `#Savetable` for autosaving results

### Device Functions
- `getDevice()`, `setOn()`, `setCurrent()`, `setVoltage()`, `readVoltage()`, `readCurrent()`, `readTemperature()`, `readCapacity()`, `readEnergy()`, `reset()`
- `nameCurrent()`, `nameVoltage()` for getting device parameter names

### Utility Functions
- `formatSI()` for SI-unit formatted output
- `timestamp()` for timestamped log entries
- `int()` for integer conversion
- `time` variable for elapsed time
- String operations: `+` operator for concatenation
- JavaScript-style syntax: `if (condition) { }` blocks for inline operations

## Safety Systems

Both scripts implement comprehensive safety monitoring:
- Voltage limits (2.5V - 4.2V range)
- Temperature limits (15°C - 50°C range)
- Time-based cutoffs
- Pre-test battery condition checks
- Automatic load shutdown on limit violations

## Data Logging

All tests generate CSV files with:
- Time-stamped measurements
- Voltage, current, temperature readings
- Calculated capacity (mAh) and energy (Wh)
- Power measurements (W)
- Internal resistance measurements (mΩ)
- Phase indicators (advanced test only)
- Real-time charting capabilities

### Logging Initialization Sequence

**Basic Test** (uses default autosave):
```
#logTime 1
#log 1
#logClear
#logStart "header,columns,..."
```

**Advanced Test** (uses named file):
```
#log 1
#logFile "filename.csv"
#logStart "header,columns,..."
```

**Critical**: Always initialize logging before starting the load, and ensure CSV headers in `#logStart` match the order of values in `#logData` calls.

## Test Phases (Advanced Script)

1. **Conditioning**: 15 minutes at constant current (1A)
2. **Ramp Up**: 15 minutes linear increase (1A → 2A)
3. **Maximum Load Hold**: 15 minutes at peak current (2A)
4. **Ramp Down**: 15 minutes linear decrease (2A → 1A)
5. **Final Discharge**: Constant current until voltage cutoff

## Common Development Tasks

When modifying these scripts:
- Test parameter changes require updating popup dialogs and validation ranges
- Safety limit modifications need updates in multiple condition checks
- Logging changes require updates to both file headers and data output statements
- New test phases should follow the existing pattern of time-based loops with safety checks

## Script Execution and Validation

These scripts are designed to run on electronic load test controllers. Key execution patterns:
- Scripts execute sequentially, line by line
- Variables persist throughout script execution
- Device state changes are immediate and affect hardware
- Safety violations immediately terminate execution via `break` or `#return`
- Always use `#delay` after device state changes to allow stabilization (typically 0.5-1 second)

### Hardware Limitations

**Temperature Sensing**: Some devices (e.g., DL24P) don't support external temperature sensors via firmware. Scripts may default to room temperature (25°C) assumptions. The basic script includes this limitation; the advanced script uses `readTemperature(load)` which may return internal device temperature or external sensor data depending on hardware capabilities.

## C-Rate Calculations and Validation

Both scripts implement C-rate validation to prevent battery damage:
```
C-rate = (Test Current in A × 1000) / Nominal Capacity in mAh
```
- **Safe ranges**: 0.5C - 1C for standard testing
- **Warning threshold**: >1C requires user confirmation
- **Critical threshold**: >2C for basic test, >3C for advanced test
- Scripts automatically calculate and display C-rates during initialization

## Internal Resistance Testing

Both scripts implement internal resistance measurements using advanced 2-point testing (inspired by Pukker's method):

**Initial Measurement** (pre-test, 2-point method):
1. Calculate 0.2C current: `nominalCapacity / 5000` (in Ampere)
2. Calculate 1C current: `nominalCapacity / 1000` (in Ampere)
3. Apply 0.2C load for 10 seconds and measure voltage
4. Ramp up to 1C load and measure voltage after 1 second
5. Calculate: `R = (V_0.2C - V_1C) / (I_1C - I_0.2C)`
6. Validate: Both currents must reach >90% of target

**Advantages of 2-point method:**
- Larger current difference = more accurate measurement
- Less susceptible to measurement noise
- Better resolution for low-resistance cells (< 50 mΩ)
- Stabilization period at 0.2C ensures accurate baseline

**Periodic Measurements** (every 5 minutes during test, basic script only):
1. Disconnect load briefly (100ms)
2. Measure rest voltage
3. Reconnect load
4. Measure loaded voltage
5. Calculate resistance from voltage difference

Results are logged in mΩ (milliohms) and used for battery health assessment:
- < 50 mΩ: Good condition
- 50-100 mΩ: Moderate aging
- > 100 mΩ: High resistance, possible aging or damage

## Script Language Debugging Patterns

Common debugging approaches for the custom test controller language:
- Use `="Debug message"` for runtime output and troubleshooting
- Add `#delay` statements after device state changes to ensure stability
- Validate device readings with bounds checking before calculations
- Use conditional blocks to prevent division by zero in resistance calculations
- Implement graceful error handling with early `#return` statements

## Safety Implementation Patterns

Safety checks follow consistent patterns across both scripts:
1. **Pre-test validation**: Check start voltage, temperature, and C-rate limits
2. **Runtime monitoring**: Continuous voltage, temperature, and time limit checks
3. **Emergency shutdown**: Immediate load disconnect and logging termination
4. **Multi-level warnings**: Notice → Warning → Critical progression

## Parameter Modification Guidelines

When updating test parameters:
- **Popup dialogs**: Update `#popupNumber` min/max ranges and default values
- **Validation logic**: Modify corresponding `#if` condition checks
- **Safety limits**: Update both warning thresholds and critical shutdown conditions
- **Logging headers**: Ensure CSV column headers match `#logData` output sequence

### Energy and Capacity Calculations

Both scripts track:
- **Capacity (mAh)**: `capacity = capacity + (current * (time_step / 3600))`
- **Energy (Wh)**: `energy = energy + (voltage * current * (time_step / 3600))`
- **Power (W)**: `power = voltage * current`
- **Average Voltage**: Calculated as `energy / capacity`

Time steps vary:
- Basic test: 1 second intervals
- Advanced test: 1 second during ramp phases, 10 seconds during constant current phases

## Community Script Standards

TestController user scripts typically follow these conventions:
- Scripts stored as "*Script.txt" in ScriptLibrary folder
- Use `#scriptMenu` for menu integration
- Include comprehensive popup configuration dialogs
- Implement autosave functionality for unattended operation
- Support multiple load types (DL24, PX100, etc.)
- Include dropout protection and failsafe mechanisms
- Follow IEC61960 standards for internal resistance testing

## Advanced Features from Pukker's Script

The included `Battery Test with IntResistance Check AutosaveTussen.txt` demonstrates advanced TestController scripting techniques:

### Internal Resistance Measurement
- Two-point measurement using 0.2C and 1C currents
- Formula: `R = (V_lowC - V_highC) / (I_highC - I_lowC)`
- Performed at beginning of test before main discharge

### Math Expressions vs Manual Calculation
- **Math expressions** (`#math`): TestController calculates automatically, accessible via `Math.Power`, `Math.Capacity`, etc.
- **Manual calculation**: Calculated in script with `=var` assignments, logged with `#logData`
- Pukker's script uses math expressions; our custom scripts use manual calculations for more control

### Autosave Workflow
1. Set `Autosave` checkbox in popup
2. Use `#Charttitle` to set output filename
3. Configure export columns with `#ExportInit` and `#ExportColumn`
4. Save with `#Savechart`, `#Savelog`, `#ExportTable`

### Safety Pattern Differences
- **Pukker's approach**: Uses `#wait` with compound conditions for continuous monitoring
- **Our approach**: Uses `#while` loops with explicit `#delay` and manual checks
- Both valid; `#wait` is more concise, `#while` provides more control

## File Structure

- `18650_test_script.txt` - Basic constant current discharge test
- `18650_advanced_test.txt` - Multi-phase advanced test with ramping
- `Battery Test with IntResistance Check AutosaveTussen.txt` - Pukker's original script with advanced features
- `README.md` - German documentation of test procedures and parameters
- `TESTING_GUIDE.md` - Comprehensive English guide for testing and qualifying cells for reuse
- `CLAUDE.md` - This file
- `LICENSE` - Project license file