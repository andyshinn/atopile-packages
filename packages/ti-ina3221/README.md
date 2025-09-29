# TI INA3221 26V, Triple Channel, 13-Bit, I2C Current and Voltage Monitor

The INA3221 is a three-channel, high-side current and bus voltage monitor with an I2C- and SMBus-compatible interface. This package provides an easy-to-use atopile module for integrating the INA3221 into your designs.

## Features

- **Triple Channel Monitoring**: Monitor current and voltage on up to 3 independent power rails
- **Wide Input Range**: Bus voltages from 0V to 26V
- **High Accuracy**: ±80µV offset voltage, 0.25% gain error (maximum)
- **Flexible Power Supply**: 2.7V to 5.5V supply voltage
- **I2C Interface**: Fast mode I2C up to 400kHz with 4 programmable addresses
- **Programmable Alerts**: Critical and warning alerts with open-drain outputs
- **Power Valid Output**: Monitors when all channels reach valid voltage levels
- **Low Power**: 350µA typical supply current

## Usage

```ato
import I2C, ElectricPower
from "ti-ina3221/ina3221.ato" import INA3221_driver

module MySystem:
    # Power supplies
    power_3v3 = new ElectricPower

    # Current monitor
    current_monitor = new INA3221_driver

    # Connect power
    current_monitor.power_3v3 ~ power_3v3
    current_monitor.power_pu ~ power_3v3

    # I2C connection
    i2c = new I2C
    i2c ~ current_monitor.i2c

    # Configure I2C address (A0 = GND -> address 0x40)
    current_monitor.addressor.address_lines[0].line ~ power_3v3.gnd

    # Connect to shunt resistors for current monitoring
    # Channel 1: Monitor 3.3V rail
    # current_monitor.ch1_in_p.line ~ supply_side_of_shunt_resistor
    # current_monitor.ch1_in_n.line ~ load_side_of_shunt_resistor
```

## Pin Configuration

### Power Supply
- **VS**: Supply voltage (2.7V to 5.5V)
- **VPU**: Pull-up supply for power valid output
- **GND**: Ground reference

### I2C Interface
- **SCL**: I2C clock line
- **SDA**: I2C data line
- **A0**: Address configuration pin

### Current Sensing Inputs
- **IN1_P/IN1_N**: Channel 1 differential shunt inputs
- **IN2_P/IN2_N**: Channel 2 differential shunt inputs
- **IN3_P/IN3_N**: Channel 3 differential shunt inputs

### Alert Outputs (Open-Drain)
- **CRITICAL**: Critical alert output
- **WARNING**: Warning alert output
- **PV**: Power valid output
- **TC**: Timing control output

## I2C Addressing

The INA3221 supports 4 different I2C addresses based on the A0 pin connection:

| A0 Connection | I2C Address |
|---------------|-------------|
| GND           | 0x40        |
| VS            | 0x41        |
| SDA           | 0x42        |
| SCL           | 0x43        |

## Register Map

The INA3221 provides extensive register access for:
- Shunt voltage readings (40µV per LSB)
- Bus voltage readings (8mV per LSB)
- Configurable conversion times and averaging
- Programmable alert limits for each channel
- Power valid threshold configuration
- Mask/enable control for alerts

## Typical Applications

- **Power Management**: Monitor multiple power rails in systems
- **Battery Monitoring**: Track current consumption and battery voltage
- **Server/Telecom**: Monitor power distribution in data centers
- **Test Equipment**: Measure current and voltage in test setups
- **Motor Control**: Monitor motor supply current

## Package Information

- **Package**: 16-pin VQFN (4.00mm × 4.00mm)
- **Operating Temperature**: -40°C to +125°C
- **ESD Rating**: ±2.5kV HBM

## Implementation Notes

1. **Decoupling**: A 100nF ceramic capacitor is included for power supply decoupling
2. **Pull-ups**: Alert outputs require external pull-up resistors
3. **Input Protection**: Consider 10Ω series resistors on inputs for high dV/dt protection
4. **Kelvin Connections**: Use 4-wire (Kelvin) connections to shunt resistors for best accuracy

## Limitations

- Maximum bus voltage: 26V (do not exceed)
- Differential shunt voltage: ±163.8mV full scale
- I2C fast mode: Up to 400kHz
- Common-mode voltage range: -0.3V to +26V

## Contributing

Contributions to this package are welcome via pull requests on the GitHub repository.

## License

This atopile package is provided under the [MIT License](https://opensource.org/license/mit/).