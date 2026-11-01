# SolaX X1-Lite-LV Modbus Fixes

This fork contains fixes for **SolaX X1-Lite-LV GEN2**, tested with an inverter using serial prefix `10J0C0`.

## Changes

### 1. Charge/Discharge time decoding fix

X1-Lite-LV stores time values in a single 16-bit register using:

```text
high byte = hour
low byte  = minute
```

Example:

```text
0x171E = 23:30
0x0600 = 06:00
```

A dedicated function was added:

```python
value_function_lv_time()
```

This avoids changing the global `value_function_gen4time`, which is also used by other inverter plugins.

---

### 2. Charge/Discharge Period 3 register fix

The original plugin reused the Period 1 registers for Period 3.

The corrected registers are:

```text
0x2588  Charge 3 Start
0x2589  Charge 3 Stop
0x258A  Discharge 3 Start
0x258B  Discharge 3 Stop
```

The fix was applied to both `SENSOR_TYPES_MAIN` and `TIME_TYPES`.

---

### 3. X1-Lite-LV model detection

Model detection by serial prefix was added/corrected:

```text
10J080 -> X1-Lite-LV 8.0kW
10J0C0 -> X1-Lite-LV 8.0kW
```

For serial prefix `10J0C0`, the inverter is reported as:

```text
X1-Lite-LV 8.0kW
```

## Tested with

```text
SolaX X1-Lite-LV
Modbus TCP -> RS485 gateway
Home Assistant
homeassistant-solax-modbus 2026.10.2
```

The register mapping was verified directly using `mbpoll`.