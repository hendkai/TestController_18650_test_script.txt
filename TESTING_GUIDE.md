# 18650 Battery Testing Guide

A comprehensive guide for testing and qualifying 18650 lithium-ion cells for safe reuse.

## Table of Contents

- [Overview](#overview)
- [Safety First](#safety-first)
- [Testing Strategy](#testing-strategy)
- [Phase 1: Pre-Test Inspection](#phase-1-pre-test-inspection)
- [Phase 2: Quick Test](#phase-2-quick-test-pukkers-script)
- [Phase 3: Detailed Test](#phase-3-detailed-test-basic-script)
- [Phase 4: Performance Test](#phase-4-performance-test-advanced-script)
- [Cell Matching for Battery Packs](#cell-matching-for-battery-packs)
- [Use Case Recommendations](#use-case-recommendations)
- [Time-Saving Tips](#time-saving-tips)

## Overview

This guide provides a systematic approach to testing salvaged or used 18650 cells for safe reuse. The multi-phase testing strategy ensures comprehensive characterization while remaining practical for large quantities of cells.

### Why Multiple Test Phases?

- **Phase 1** eliminates obviously damaged cells (visual inspection)
- **Phase 2** quickly identifies capacity and internal resistance (2-4 hours)
- **Phase 3** provides detailed degradation analysis (2-4 hours)
- **Phase 4** tests high-drain performance for demanding applications (optional)

## Safety First

### ⚠️ Critical Safety Rules

**Never compromise on safety when testing lithium-ion cells!**

- ✅ Always test on non-flammable surfaces (ceramic, metal, concrete)
- ✅ Use LiPo safety bags for testing
- ✅ Keep smoke detector nearby
- ✅ Never leave tests unattended overnight
- ✅ Test at room temperature (15-25°C / 59-77°F)
- ✅ Stop test immediately if cell exceeds 50°C (122°F)
- ✅ Have fire extinguisher accessible (Class D for lithium fires)

**Dispose of cells immediately if:**
- Voltage < 2.5V (deep discharge)
- Voltage > 4.3V (overcharge)
- Physical damage (dents, rust, leaks)
- Bulging or swelling
- Excessive heat during charging (>45°C)
- Strong odor or leakage

## Testing Strategy

### Recommended Test Flow

```
┌─────────────────────────────────────────────────────┐
│  Phase 1: Visual Inspection & Voltage Check         │
│  Time: 1-2 minutes per cell                         │
│  Reject: ~50% of salvaged cells                     │
└────────────────┬────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────┐
│  Phase 2: Quick Test (Pukker's Script)              │
│  Time: 2-4 hours per cell                           │
│  Measures: Capacity, Internal Resistance            │
│  Reject: ~30% of remaining cells                    │
└────────────────┬────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────┐
│  Phase 3: Detailed Test (Basic Script)              │
│  Time: 2-4 hours per cell                           │
│  Measures: Degradation, Voltage stability           │
│  Reject: ~15% of remaining cells                    │
└────────────────┬────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────┐
│  Phase 4: Performance Test (Advanced Script)        │
│  Time: 6-8 hours per cell                           │
│  For high-drain applications only (optional)        │
│  Reject: ~10% of remaining cells                    │
└─────────────────────────────────────────────────────┘
```

## Phase 1: Pre-Test Inspection

**Required Tools:**
- Digital multimeter
- Magnifying glass or good lighting
- LiPo charger (0.5C maximum)

### Step 1: Visual Inspection

**Check for physical damage:**
- [ ] No dents, bulges, or deformations
- [ ] No rust or corrosion on terminals
- [ ] Insulator ring intact and not torn
- [ ] No leakage or residue
- [ ] Wrapper in good condition (no tears exposing metal)

**→ If ANY damage is found: DISCARD IMMEDIATELY**

### Step 2: Voltage Measurement

**Use multimeter to measure open-circuit voltage:**

| Voltage Range | Status | Action |
|---------------|--------|--------|
| < 2.5V | Deep discharged | **DISCARD** - Risk of internal damage |
| 2.5V - 3.0V | Critically low | Charge carefully, monitor closely |
| 3.0V - 4.2V | Normal | OK for testing |
| > 4.3V | Overcharged | **DISCARD** - Safety risk |

### Step 3: Charging

**Before testing, fully charge cells:**
- Charge to 4.2V at 0.5C maximum
- Monitor temperature during charging
- If cell exceeds 45°C during charging: **DISCARD**
- Allow 30 minutes rest after charging before testing

**Expected rejection rate after Phase 1: 40-60%**

## Phase 2: Quick Test (Pukker's Script)

**Purpose:** Rapid screening for capacity and internal resistance

**Script:** `Battery Test with IntResistance Check AutosaveTussen.txt`

### Recommended Settings

```
Nominal Voltage:           3.7V
Minimum Voltage:           2.5V
Discharge Current:         0.5-1.0A (0.5C recommended)
Rated Battery Capacity:    2500 mAh (conservative estimate)
Internal Resistance Check: ✅ ENABLED
Logging Interval:          10 seconds
Time Limited Discharge:    ✅ ENABLED
Max Discharge Time:        300 minutes
Max Temperature:           50°C
Autosave:                  ✅ ENABLED
Battery Name:              Cell ID (e.g., "A001")
```

### Test Duration

- **Typical:** 2-4 hours per cell
- **Run parallel tests** if multiple loads available

### Acceptance Criteria

#### Internal Resistance (IR)

| IR Value | Quality | Suitable For |
|----------|---------|--------------|
| < 50 mΩ | Excellent | High-drain applications |
| 50-70 mΩ | Good | Medium-drain applications |
| 70-100 mΩ | Fair | Low-drain applications only |
| > 100 mΩ | Poor | **DISCARD** |

#### Capacity

| Capacity vs Nominal | Health | Action |
|---------------------|--------|--------|
| > 80% | Excellent | Proceed to Phase 3 |
| 70-80% | Good | Proceed to Phase 3 |
| 60-70% | Fair | Low-drain use only |
| < 60% | Poor | **DISCARD** |

### Data to Record

Save the following data for each cell:
- Cell ID
- Measured capacity (mAh)
- Initial internal resistance (mΩ)
- Energy (Wh)
- Test date

**Expected rejection rate: 20-40%**

## Phase 3: Detailed Test (Basic Script)

**Purpose:** Detailed characterization with degradation monitoring

**Script:** `18650_test_script.txt`

**When to use:** For cells that passed Phase 2 with Good or Excellent ratings

### Recommended Settings

```
Nominal Capacity:          [Use measured value from Phase 2]
Discharge Current:         1.0-1.5A (1C rate)
Minimum Voltage:           2.5V
Start Voltage:             3.7V
Maximum Test Time:         180 minutes
Maximum Temperature:       50°C
Minimum Temperature:       15°C
```

### Test Duration

- **Typical:** 2-4 hours per cell
- **1 second logging interval** provides detailed data

### What This Test Reveals

#### Periodic IR Measurements (Every 5 Minutes)

**Tracks internal resistance changes during discharge:**
- Healthy cells: IR increases < 10%
- Aging cells: IR increases 10-20%
- Degraded cells: IR increases > 20%

**→ Reject if IR increases > 20% during test**

#### Voltage Drop Rate Monitoring

**Detects weak cells through voltage stability:**
- Normal: Gradual voltage decline
- Warning: Sudden voltage drops
- Critical: Voltage drop rate > 0.1 V/min

**→ Reject if voltage drop rate warnings occur**

#### Energy Tracking

**Provides realistic usable capacity:**
- Energy (Wh) = Voltage × Current × Time
- Average voltage = Energy / Capacity
- Healthy 18650: ~3.6V average during discharge

#### Battery Health Assessment

The script provides automatic health grading:
- **GOOD:** > 80% of nominal capacity
- **FAIR:** 60-80% of nominal capacity
- **POOR:** < 60% of nominal capacity

### Acceptance Criteria for Phase 3

**Pass to reuse if:**
- ✅ Battery Health: GOOD or FAIR
- ✅ IR increase during test < 20%
- ✅ No voltage drop rate warnings
- ✅ No temperature limit violations
- ✅ Smooth discharge curve (visual inspection of log)

**Expected rejection rate: 10-20%**

## Phase 4: Performance Test (Advanced Script)

**Purpose:** Stress testing for high-drain applications

**Script:** `18650_advanced_test.txt`

**When to use:** OPTIONAL - Only for cells intended for high-drain applications (e-bikes, power tools, RC vehicles)

### Recommended Settings

```
Nominal Capacity:          [Use measured value from Phase 2/3]
Initial Current:           1.0A
Ramp Start Current:        1.0A
Ramp Maximum Current:      2.0-3.0A (depending on application)
Ramp Duration:             15 minutes
Minimum Voltage:           2.5V
Start Voltage:             3.7V
Maximum Temperature:       50°C
Minimum Temperature:       15°C
```

### Test Phases

The advanced test consists of 5 phases:

1. **Conditioning (15 min):** Constant 1A discharge to stabilize cell
2. **Ramp Up (15 min):** Linear increase from 1A to maximum current
3. **Maximum Load Hold (15 min):** Constant discharge at peak current
4. **Ramp Down (15 min):** Linear decrease back to 1A
5. **Final Discharge:** Continue at 1A until minimum voltage

### Test Duration

- **Total:** 6-8 hours per cell
- **Critical phase:** Ramp up and hold (watch for failures)

### What This Test Reveals

#### Dynamic Load Response

- High-quality cells: Voltage remains stable during ramps
- Degraded cells: Voltage drops sharply under high load

#### Thermal Performance

- Monitor temperature during high-current phases
- Good cells: Temperature < 45°C even at 2-3A
- Weak cells: Rapid temperature increase

#### Internal Resistance Under Load

- Periodic measurements during hold phase
- Resistance should remain relatively stable
- Significant increase indicates cell degradation

### Acceptance Criteria for Phase 4

**Pass for high-drain use if:**
- ✅ Completes all phases without shutdown
- ✅ Temperature stays < 45°C during high-current phases
- ✅ Voltage doesn't collapse during ramp-up (> 3.0V sustained)
- ✅ IR remains stable during maximum load hold
- ✅ Total capacity > 80% of nominal

**→ Cells passing Phase 4 are suitable for demanding applications**

**Expected rejection rate: 5-15%**

## Cell Matching for Battery Packs

### Why Matching Matters

When building battery packs with multiple cells in series or parallel:
- **Unmatched cells** lead to uneven discharge/charge rates
- **Weak cells** limit pack performance
- **Mismatched IR** causes uneven heating
- **Capacity differences** reduce overall pack capacity

### Matching Criteria

For cells to be used in the same pack:

| Parameter | Tolerance | Critical? |
|-----------|-----------|-----------|
| Capacity | ±50 mAh (±2%) | ✅ YES |
| Internal Resistance | ±5 mΩ | ✅ YES |
| Initial Voltage | ±10 mV | ⚠️ Moderate |
| Age/Cycle Count | Same batch preferred | ⚠️ Moderate |
| Brand/Model | Identical | ✅ YES |

### Matching Procedure

1. **Complete Phase 2 + 3** for all candidate cells
2. **Create a spreadsheet** with test results:

```
Cell_ID | Capacity | IR_Initial | IR_Final | Health | Notes
--------|----------|------------|----------|--------|-------
A001    | 2850 mAh | 45 mΩ      | 48 mΩ    | GOOD   | Pass
A002    | 2830 mAh | 44 mΩ      | 47 mΩ    | GOOD   | Pass
A003    | 2420 mAh | 65 mΩ      | 72 mΩ    | FAIR   | Low-drain only
A004    | 2860 mAh | 46 mΩ      | 49 mΩ    | GOOD   | Pass
```

3. **Sort by capacity** and group cells within ±50 mAh
4. **Within each group**, match by IR within ±5 mΩ
5. **Label matched groups** (e.g., "Group A", "Group B")

### Pack Configuration Guidelines

**Series Connection (voltage increase):**
- Match tolerance: ±1% capacity, ±3 mΩ IR
- Weakest cell limits the pack
- Use BMS (Battery Management System)

**Parallel Connection (capacity increase):**
- Match tolerance: ±2% capacity, ±5 mΩ IR
- Cells self-balance, more forgiving
- Still recommend matched cells

**Series + Parallel (e.g., 3S2P = 12.6V pack):**
- Match parallel groups first (±2%)
- Then match series groups (±1%)
- Most demanding configuration

## Use Case Recommendations

### Scenario A: Low-Drain Applications
**Examples:** Flashlights, remote controls, wall clocks, emergency lights

**Required Testing:**
```
Phase 1 + Phase 2 (Pukker's Script)
```

**Acceptance Criteria:**
- Internal Resistance: < 100 mΩ
- Capacity: > 60% of nominal
- No physical damage

**Time Investment:** ~3 hours per cell

**Expected Yield:** 40-60% of salvaged cells

---

### Scenario B: Medium-Drain Applications
**Examples:** Power banks, USB battery packs, portable speakers, LED panels

**Required Testing:**
```
Phase 1 + Phase 2 + Phase 3 (Basic Script)
```

**Acceptance Criteria:**
- Internal Resistance: < 70 mΩ
- Capacity: > 70% of nominal
- No voltage drop warnings
- Battery Health: GOOD or FAIR

**Time Investment:** ~5-6 hours per cell

**Expected Yield:** 25-40% of salvaged cells

---

### Scenario C: High-Drain Applications
**Examples:** E-bikes, power tools, RC vehicles, electric skateboards, drones

**Required Testing:**
```
Phase 1 + Phase 2 + Phase 3 + Phase 4 (Advanced Script)
```

**Acceptance Criteria:**
- Internal Resistance: < 50 mΩ
- Capacity: > 80% of nominal
- Passes all ramp phases without issues
- Temperature < 45°C under load
- Battery Health: GOOD (Excellent preferred)

**Time Investment:** ~8-10 hours per cell

**Expected Yield:** 10-25% of salvaged cells

---

### Scenario D: Battery Packs (Any Application)
**Examples:** Solar storage, UPS backup, DIY Powerwall, e-bike/e-scooter packs

**Required Testing:**
```
Phase 1 + Phase 2 + Phase 3
+ Cell Matching
+ (Optional: Phase 4 for high-drain packs)
```

**Additional Requirements:**
- All cells from same test batch
- Matched within tolerances (see Matching section)
- BMS (Battery Management System) required
- Proper pack assembly and insulation

**Time Investment:** ~6-10 hours per cell + matching time

**Expected Yield:** 20-35% of salvaged cells (in matched groups)

## Time-Saving Tips

### For Large Quantities

**1. Batch Visual Inspection**
- Process 50-100 cells at once
- Reject ~50% immediately based on visual/voltage
- **Saves:** 70% of total testing time

**2. Parallel Testing**
- Use multiple electronic loads simultaneously
- Test 4-8 cells overnight with autosave enabled
- **Speeds up:** 4-8× faster throughput

**3. Prioritize Promising Cells**
- Test cells from known good sources first
- Skip Phase 4 unless specifically needed
- **Saves:** 30-40% of testing time

**4. Overnight Testing**
- Run Pukker's script with autosave overnight
- Safe for Phase 2 testing (4.2V → 2.5V at 0.5-1A)
- Check results in the morning
- **Maximizes:** Equipment utilization

### Realistic Time Estimates

**Processing 100 salvaged cells:**

| Phase | Active Time | Cell Yield | Total Time |
|-------|-------------|------------|------------|
| Phase 1 (Visual) | 1-2 hours | 50 cells | 2 hours |
| Phase 2 (Quick) | 3h × 50 cells | 30 cells | 2 days (parallel) |
| Phase 3 (Detail) | 4h × 30 cells | 25 cells | 2 days (parallel) |
| Matching | 2 hours | 20 cells (4-5 groups) | 2 hours |

**Total:** ~4-5 days of testing to get 20-25 qualified cells

**If testing single-threaded:** Would take 150-200 hours (unfeasible!)

## Best Practices

### Documentation

**Create a testing log for each cell:**
```
Cell ID: A001
Source: Laptop battery pack (Dell)
Date Tested: 2025-01-15
Phase 2 Results:
  - Capacity: 2850 mAh
  - IR Initial: 45 mΩ
  - Energy: 10.2 Wh
  - Status: PASS
Phase 3 Results:
  - IR Final: 48 mΩ
  - Health: GOOD
  - Status: PASS
Matched Group: Group A
Notes: Excellent cell, suitable for high-drain
```

### Cell Labeling

**After testing, label each cell:**
- Cell ID (permanent marker or label)
- Measured capacity
- Test date
- Matched group (if applicable)

**Example:** `A001 | 2850mAh | 2025-01 | Grp-A`

### Storage

**Store tested cells properly:**
- Voltage: 3.7-3.8V (storage voltage)
- Temperature: Cool, dry place (15-25°C)
- Insulated: Prevent short circuits
- Separated: By groups/ratings

### Re-Testing

**Periodic re-testing recommended:**
- Every 6-12 months for stored cells
- After 50-100 charge cycles for active use
- Especially for safety-critical applications

## Resources

### Scripts in This Repository

- `18650_test_script.txt` - Basic discharge test with periodic IR measurements
- `18650_advanced_test.txt` - Multi-phase performance test with dynamic loading
- `Battery Test with IntResistance Check AutosaveTussen.txt` - Pukker's original script with autosave

### External Resources

- [TestController User Scripts by Pukker](https://lygte-info.dk/project/TestControllerUserScripts1%20UK.html#Battery_test_with_DL24/PX100_loads_by_Pukker)
- [Battery University - 18650 Guide](https://batteryuniversity.com/)
- Safety guidelines for lithium-ion handling

## Contributing

Found an error or have suggestions? Please open an issue or submit a pull request!

## License

See LICENSE file in repository root.

---

**⚠️ Disclaimer:** Battery testing involves risks. Always follow safety precautions and local regulations. The authors are not responsible for any damage or injury resulting from the use of these scripts or testing procedures.
