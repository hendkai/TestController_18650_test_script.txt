# 18650 Battery Test Scripts

This collection contains two specialized test scripts for 18650 lithium-ion batteries.

## Recent Updates

### Standard Test Script Updates (`18650_test_script.txt`)
- Added configurable start voltage check (3.5V - 4.2V)
- Improved temperature monitoring with min/max limits
- Enhanced data logging with 1-second intervals for first 5 minutes
- Added detailed temperature reporting
- GUI improvements for decimal value inputs

### New Advanced Test Features (`18650_advanced_test.txt`)
The new advanced test combines capacity testing with dynamic load ramping in three phases:

1. **Initial Constant Current Phase**
   - Standard discharge at user-defined current (typically 1C)
   - Continuous monitoring of voltage and temperature
   - Data logging at 10-second intervals

2. **Ramp Test Phase (15 minutes)**
   - Starts at defined initial current
   - Gradually increases to maximum current
   - High-resolution logging (1-second intervals)
   - Configurable maximum current up to 4.0A (0.1A steps)
   - Monitors cell behavior under increasing load

3. **Final Constant Current Phase**
   - Returns to initial current
   - Continues until cutoff voltage
   - Complete capacity measurement

This three-phase approach allows:
- Standard capacity testing
- Performance testing under increasing load
- Temperature behavior analysis
- Internal resistance estimation
- Complete discharge profile

## Available Tests

### 1. Standard Discharge Test (`18650_test_script.txt`)
A basic capacity and discharge test featuring:
- Configurable discharge current (0.1A - 2.0A)
- Adjustable minimum voltage (2.5V - 3.0V)
- Adjustable start voltage (3.5V - 4.2V)
- Temperature monitoring (10°C - 60°C)
- Automatic data logging
- Capacity calculation

### 2. Advanced Test with Ramping (`18650_advanced_test.txt`)
An advanced three-phase test:
1. **Initial Constant Current**: Normal discharge test
2. **Ramp Phase (15 min)**: Increasing current for stress testing
3. **Final Constant Current**: Concluding discharge test

Features:
- Configurable start and end currents for ramping
- Adjustable ramp duration (5-30 min, Default: 15 min)
- Adjustable start voltage (3.5V - 4.2V)
- Detailed temperature monitoring
- Comprehensive data logging
- Automatic safety shutdown

## Adjustable Parameters

### Standard Test
- Discharge current (0.1A - 2.0A)
- Minimum voltage (2.5V - 3.0V)
- Start voltage (3.5V - 4.2V)
- Maximum test time (30-240 min)
- Temperature limits (10°C - 60°C)

### Advanced Test
- Initial current (0.5A - 2.0A)
- Ramp start current (0.5A - 2.0A)
- Ramp maximum current (1.0A - 4.0A, 0.1A steps)
- Ramp duration (5-30 min)
- Start voltage (3.5V - 4.2V)
- Minimum voltage (2.5V - 3.0V)
- Temperature limits (10°C - 60°C)

## Data Logging
Both tests automatically record:
- Voltage
- Current
- Temperature
- Capacity
- Test time

## Safety Features
- Start voltage verification
- Temperature monitoring
- Low voltage cutoff
- Maximum test time limit

## Recommended Usage
- Standard Test: For basic capacity measurements
- Advanced Test: For detailed performance analysis and stress testing
