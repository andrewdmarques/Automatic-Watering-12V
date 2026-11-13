
# ESP32 Automated Plant Watering System

### 12V Submersible Pump Controller | 24-Hour Timer

## Overview

An automated plant watering controller using an ESP32 DevKit V1 and a 12V DC submersible pump.

The controller provides **14 selectable watering durations**, ranging from **15 seconds to 14 minutes**, using individual GPIO outputs. The desired watering duration is selected by connecting the pump's switching circuit to the corresponding GPIO pin.

The system operates on a repeating **24-hour cycle** without requiring Wi-Fi, internet access, or an external real-time clock.

## Hardware Requirements

- ESP32 DevKit V1 (ESP32-WROOM-32)
- 12V DC submersible water pump
- 12V DC power supply
- Logic-level MOSFET or compatible relay module
- Appropriate power supply or regulator for the ESP32
- Water reservoir and tubing
- Connecting wires

**Important:** ESP32 GPIO outputs operate at 3.3V and cannot directly power a 12V pump. A suitable MOSFET or relay circuit is required.

## Operating Sequence

1. **Power ON:** All selected GPIO outputs initialize LOW.
2. **Startup delay:** Wait 5 seconds.
3. **Activation:** All 14 GPIO outputs turn HIGH simultaneously.
4. **Timed operation:** Each GPIO turns LOW after its assigned duration.
5. **Standby:** All GPIO outputs remain LOW for the rest of the day.
6. **Repeat:** The sequence restarts every 24 hours.

Only the GPIO connected to the pump's switching circuit determines the watering duration.

## GPIO Pin Assignments

| GPIO Pin | Watering Duration |
|:---------|------------------:|
| GPIO16 (RX2) | 15 seconds |
| GPIO17 (TX2) | 30 seconds |
| GPIO18 | 45 seconds |
| GPIO19 | 1 minute |
| GPIO21 | 1 minute 30 seconds |
| GPIO22 | 2 minutes |
| GPIO23 | 3 minutes |
| GPIO25 | 4 minutes |
| GPIO26 | 5 minutes |
| GPIO27 | 6 minutes |
| GPIO32 | 8 minutes |
| GPIO33 | 10 minutes |
| GPIO13 | 12 minutes |
| GPIO14 | 14 minutes |

The selected outputs avoid the ESP32's boot-strapping pins, flash memory pins, and primary serial programming pins.

## Selecting a Watering Duration

Connect the control input of the pump's MOSFET or relay circuit to the desired GPIO pin.

**Examples:**

- GPIO16: 15 seconds of watering per day
- GPIO23: 3 minutes of watering per day
- GPIO26: 5 minutes of watering per day
- GPIO14: 14 minutes of watering per day

No software changes are required to select another predefined duration.


## Watering Volume Calculation

Water delivery depends on the pump's flow rate and operating duration.

$$
V = Q \times t
$$

Where:

- $V$ = Water delivered (L)
- $Q$ = Pump flow rate (L/min)
- $t$ = Pump operating time (minutes)

### Example: 19 L/min Submersible Pump

Assuming a constant pump flow rate of **19 liters per minute (5.02 gallons per minute)**:

| GPIO Pin | Watering Duration | Water Delivered (L) | Water Delivered (US gal) |
|:---------|:------------------|--------------------:|-------------------------:|
| GPIO16 | 15 seconds | 4.75 | 1.25 |
| GPIO17 | 30 seconds | 9.50 | 2.51 |
| GPIO18 | 45 seconds | 14.25 | 3.76 |
| GPIO19 | 1 minute | 19.00 | 5.02 |
| GPIO21 | 1.5 minutes | 28.50 | 7.53 |
| GPIO22 | 2 minutes | 38.00 | 10.04 |
| GPIO23 | 3 minutes | 57.00 | 15.06 |
| GPIO25 | 4 minutes | 76.00 | 20.08 |
| GPIO26 | 5 minutes | 95.00 | 25.10 |
| GPIO27 | 6 minutes | 114.00 | 30.11 |
| GPIO32 | 8 minutes | 152.00 | 40.15 |
| GPIO33 | 10 minutes | 190.00 | 50.19 |
| GPIO13 | 12 minutes | 228.00 | 60.23 |
| GPIO14 | 14 minutes | 266.00 | 70.27 |

*These values assume a constant flow rate of 19 L/min. Actual water delivery will vary depending on pumping height, tubing diameter, restrictions, and pump performance. Measure the installed pump's actual flow rate for accurate watering volumes.*


## Software

**Platform:** Arduino IDE

**Board:** ESP32 Dev Module

**Framework:** Arduino ESP32

**Additional libraries:** None

Upload the Arduino sketch to the ESP32 using a USB connection.

## Timing Specifications

| Parameter | Value |
|:----------|:------|
| Startup delay | 5 seconds |
| Cycle interval | 24 hours |
| Minimum watering duration | 15 seconds |
| Maximum watering duration | 14 minutes |
| Available GPIO outputs | 14 |
| GPIO HIGH voltage | 3.3V |
| Pump operating voltage | 12V DC |
| Internet required | No |
| External clock required | No |

## Important Limitations

- The 24-hour interval is measured from ESP32 startup.
- Power interruptions restart the watering schedule.
- Timing is based on the internal `millis()` counter and may drift.
- The ESP32 must remain powered continuously.
- GPIO output states are not guaranteed during the initial hardware boot period.
- The controller does not measure soil moisture or water delivery.

## Electrical Safety

- Never connect a 12V pump directly to an ESP32 GPIO pin.
- Use an appropriately rated MOSFET or relay circuit.
- Provide inductive-load protection for the pump.
- Use a suitable gate pull-down or relay input circuit to keep the pump OFF during startup.
- Keep electrical connections protected from moisture.
- Avoid operating the pump without water.
- Ensure the power supply supports the pump's startup and running current.

## Future Improvements

- Soil moisture monitoring
- Reservoir water-level detection
- Adjustable watering schedules
- Real-time clock integration
- Wi-Fi-based configuration
- Automatic dry-run protection

## Summary

This project provides a low-cost automated watering controller for 12V submersible pumps. By selecting one of 14 GPIO outputs, users can choose a predefined daily watering duration without modifying the software.

The controller operates independently and repeats the watering cycle approximately every 24 hours.
