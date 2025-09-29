# Texas Instruments BQ25798 Buck-Boost Battery Charger

The BQ25798 is a highly integrated buck-boost charger for 1-4 cell Li-Ion and Li-polymer batteries with dual-input selector and maximum power point tracking (MPPT) for small photovoltaic panels. It supports USB PD 3.0 profile and communicates through I2C interface.

## Features

- 5A buck-boost battery charger with 10mA resolution
- Input voltage range: 3.6V to 24V (absolute maximum 30V)
- Battery voltage range: 2.8V to 18.8V (1-4 cell)
- OTG output voltage: 2.8V to 22V
- I2C controlled (address 0x6B)
- USB PD 3.0 support
- Dual-input power multiplexer (VBUS + VAC1/VAC2)
- Maximum Power Point Tracking (MPPT) for solar panels
- Integrated four switching MOSFETs and BATFET
- 96.5% efficiency at optimal conditions
- Switching frequencies: 750 kHz or 1.5 MHz
- Low quiescent current: 17 µA in battery-only mode
- 500 nA in Charger Shutdown Mode
- 29-pin QFN package (4mm x 4mm)

## Usage

```ato
from "ti-bq25798/bq25798.ato" import BQ25798_driver

module MyCharger:
    charger = new BQ25798_driver

    # Power connections
    usb_input = new ElectricPower
    solar_input = new ElectricPower
    battery_pack = new ElectricPower
    system_rail = new ElectricPower

    # I2C for control
    control_bus = new I2C

    # External inductor for buck-boost operation
    main_inductor = new Inductor
    main_inductor.inductance = 2.2uH +/- 20%

    # Connect interfaces
    usb_input ~ charger.power_vbus
    solar_input ~ charger.power_vac1
    battery_pack ~ charger.power_battery
    system_rail ~ charger.power_system
    control_bus ~ charger.i2c

    # Connect external inductor
    charger.switch1 ~ main_inductor.unnamed[0]
    charger.switch2 ~ main_inductor.unnamed[1]
```

## Power Interfaces

- `power_vbus`: USB/adapter input power (3.6V to 24V)
- `power_vac1`: Solar panel or auxiliary input 1 (3.6V to 24V)
- `power_vac2`: Auxiliary input 2 (3.6V to 24V)
- `power_battery`: Battery connection (2.8V to 18.8V, 1-4 cell)
- `power_system`: System output power (2.8V to 22V)
- `power_regn`: Internal 3.3V LDO output
- `power_pmid`: Power path multiplexer output

## Communication

- `i2c`: I2C interface for device control and monitoring (address 0x6B)

## Control Pins

- `charge_enable`: Charge enable control (active low)
- `power_on`: System power on control (active low)
- `input_current_limit`: Input current limit / Hi-Z control
- `charge_status`: Charge status output
- `interrupt`: Interrupt output (active low)

## USB Interface

- `usb_dp`: USB D+ data line (for USB PD communication)
- `usb_dm`: USB D- data line (for USB PD communication)

## External Components

- `switch1`, `switch2`: Switching nodes for external inductor connection
- `bootstrap1`, `bootstrap2`: Bootstrap capacitor connections
- `ac_drive1`, `ac_drive2`: AC drive pins
- `source_drive`: Source drive pin

## Additional Features

- `thermal_sense`: Temperature sensing input for thermal regulation
- `program_current`: Programming current setting
- `battery_positive`: Battery positive terminal sensing
- Integrated decoupling capacitors for VBUS, REGN, and SYS rails
- Bootstrap capacitors for switching MOSFETs

## Package Requirements

This package includes the BQ25798RQMR component (29-pin QFN) with symbol, footprint, and 3D model from JLCPCB/LCSC (part number C2876593).

## Contributing

Contributions to this package are welcome via pull requests on the GitHub repository.

## License

This atopile package is provided under the [MIT License](https://opensource.org/license/mit/).