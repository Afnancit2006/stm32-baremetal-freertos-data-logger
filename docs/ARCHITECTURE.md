# Planned Architecture

## Design goals

- Keep board-specific configuration separate from reusable driver logic.
- Keep interrupt service routines short and deterministic.
- Move blocking storage work into a dedicated logger task.
- Pass acquired data through explicit RTOS communication objects.
- Make hardware-independent record and buffer logic testable on a host computer.

## Proposed module boundaries

| Module | Responsibility |
|---|---|
| Board support | Clock tree, pin selection, interrupt mapping, and hardware revision details |
| Peripheral drivers | GPIO, UART, SPI, I2C, CAN, ADC, timer/PWM, and watchdog control |
| Flash driver | W25Q commands, timing, bounds checks, and error status |
| Buffer utilities | Circular byte buffer and fixed-size record queue logic |
| Acquisition task | Periodic sampling or generation of test data |
| Logger task | Record creation, buffering, and flash persistence |
| Command task | UART commands for status, readout, and diagnostics |
| Health monitor | Task liveness checks, fault reporting, and watchdog policy |

## Interrupt design rules

1. Clear and record the interrupt source promptly.
2. Move data into a bounded buffer or notify a task.
3. Use only RTOS APIs explicitly documented as ISR-safe.
4. Avoid dynamic allocation, formatted printing, and long polling loops inside an ISR.
5. Record overflow and error conditions instead of silently discarding them.

## Data record draft

The final layout will be chosen only after the logging requirements are confirmed. A likely record contains a format version, sequence number, timestamp or tick count, source identifier, payload length, payload, status flags, and an integrity check. Byte order, alignment, packing, and recovery behaviour must be documented before implementation.

## Open decisions

- Exact board, pins, oscillator, and clock configuration.
- Exact W25Q part number and capacity.
- Whether timestamps come from an RTC, RTOS ticks, or an external time source.
- Record size and retention target.
- DMA requirements and whether they improve the measured workload.
