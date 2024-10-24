# GorillaCAN Firmware v0.1

[Hardware v0.1](/gorilla_can/GorillaCAN%20Datasheet%20-%20Hardware%20v0.1%20Rev%20A.pdf)

## Warning
When connected to vehicles, CAN networks pose danger if not properly configured. GorillaCAN is currently a development product. Use at your own risk. No liability for claims, damages, etc is implied.

## Supported Features

- Configurable input "SENS_IN"
    - Digital 0/5V output
    - 10 bit ADC input (0-5V)
    - Frequency input (edge detection)

    - All modes: fixed 10Hz sampling frequency
    - Transmit message configurable
- Digital output "SENS_OUT"
    - Recieve message configurable
- 500kBaud CAN speed (configurability planned)

## Configuring GorillaCAN

Gorilla CAN can be configured over a CAN connection. In the current version, configuration packets will apply to every connected GorillaCAN module. To configure modules individually, a direct connection is required.

To query the current configuration, send message `CAN_MSG_CONFIG` with magicword `0xE144C9F6`. The module will reply with `CAN_MSG_CONFIG_QUERY`.

To reset the module to defaults, send message `CAN_MSG_CONFIG` with magicword `0xD133C1F0`. Setting will be updated on next power-cycle.

To configure the module, send message `CAN_MSG_CONFIG` with magicword `0xE044C8F6`, specifying settings according to the format below.

## Packet Formats


### `GCAN_SENS_IN`: reading from pin SENS_IN

All multi-byte signals to be sent in Intel/little-endian order

Digital Mode:
| ID     | `0x210` (default) |
| ------ | ----------------- |
| DLC    | 1                 |
| UNUSED | 7 bits            |
| STATE  | 1 bit             |

Analog Mode:
| ID    | `0x210` (default) |
| ----- | ----------------- |
| DLC   | 2                 |
| STATE | uint16            |
|       | `0` = 0.0V        |
|       | `1023` = 5.0V     |

Frequency Mode:
| ID    | `0x210` (default)      |
| ----- | ---------------------- |
| DLC   | 4                      |
| STATE | uint32                 |
|       | Frequency in Hz to 0dp |


### `GCAN_SENS_OUT`: command to pin SENS_OUT

| ID     | `0x211` (default) |
| ------ | ----------------- |
| DLC    | 1                 |
| UNUSED | 7 bits            |
| STATE  | 1 bit             |

### `GCAN_MSG_CONFIG`: query, configure or reset GorillaCAN

Query command:
| ID         | `0x661` (default)           |
| ---------- | --------------------------- |
| DLC        | 4                           |
| MAGIC_WORD | `0xE144C9F6` literal uint32 |

Reset command:
| ID         | `0x661` (default)           |
| ---------- | --------------------------- |
| DLC        | 4                           |
| MAGIC_WORD | `0xD133C1F0` literal uint32 |

Query command:
| ID              | `0x661` (default)            |
| --------------- | ---------------------------- |
| DLC             | 7                            |
| MAGIC_WORD      | `0xE044C8F6` literal, uint32 |
| SENS_IN_MODE    | enum, uint8                  |
|                 | 0 = Disabled                 |
|                 | 1 = Digital                  |
|                 | 2 = Analog                   |
|                 | 3 = Frequency                |
| SENS_OUT_MSG_ID | uint8                        |
| SENS_IN_MSG_ID  | uint8                        |


Note: in version v0.1 SENS_OUT_MSG_ID/SENS_IN_MSG_ID can only be set in the range 0-255

### `GCAN_MSG_CONFIG_QUERY`: sent in response to config query

| ID              | `0x661` (default) |
| --------------- | ----------------- |
| DLC             | 3                 |
| SENS_IN_MODE    | enum, uint8       |
|                 | 0 = Disabled      |
|                 | 1 = Digital       |
|                 | 2 = Analog        |
|                 | 3 = Frequency     |
| SENS_OUT_MSG_ID | uint8             |
| SENS_IN_MSG_ID  | uint8             |
