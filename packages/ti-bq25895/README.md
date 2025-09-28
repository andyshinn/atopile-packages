# Texas Instruments BQ25895 5A Switch-Mode Battery Charger

The BQ25895 is a highly integrated switch-mode battery charge management and system power path management device for single cell Li-Ion and Li-polymer batteries. It supports high input voltage fast chargers and communicates through I2C interface.

## Features

- 5A switch-mode battery charger
- Input voltage range: 3.9V to 14V
- Battery voltage range: 3.0V to 4.5V
- I2C controlled (address 0x6A)
- USB OTG (On-The-Go) boost mode: 4.5V to 5.5V output
- MaxCharge USB detection
- Input current optimization (ICO)
- Power path management with NVDC architecture
- Thermal regulation and protection
- 24-pin WQFN package

## Usage

```ato
from "ti-bq25895/bq25895.ato" import BQ25895_driver

module MyCharger:
    charger = new BQ25895_driver

    # Power connections
    usb_input = new ElectricPower
    battery_pack = new ElectricPower
    system_rail = new ElectricPower

    # I2C for control
    control_bus = new I2C

    # Connect interfaces
    usb_input ~ charger.power_vbus
    battery_pack ~ charger.power_battery
    system_rail ~ charger.power_system
    control_bus ~ charger.i2c
```

## Power Interfaces

- `power_vbus`: USB/adapter input power (3.9V to 14V)
- `power_battery`: Battery connection (3.0V to 4.5V)
- `power_system`: System output power (3.5V to 14V)
- `power_regn`: Internal 3.3V LDO output

## Communication

- `i2c`: I2C interface for device control and monitoring (address 0x6A)

## Control Pins

- `charge_enable`: Charge enable control
- `otg_enable`: OTG boost mode enable
- `input_select`: Input current limit selection
- `charge_status`: Charge status output
- `interrupt`: Interrupt output
- `power_on`: System power on control

## USB Interface

- `usb_dp`: USB D+ data line
- `usb_dm`: USB D- data line

## Additional Features

- `thermal_sense`: Temperature sensing input
- `current_limit`: Input current limit setting
- Integrated decoupling capacitors for VBUS and REGN rails

## Package Requirements

This package requires you to create the BQ25895 component part, symbol, and footprint. The component uses a 24-pin WQFN package.

## Contributing

Contributions to this package are welcome via pull requests on the GitHub repository.

## License

This atopile package is provided under the [MIT License](https://opensource.org/license/mit/).