# Development Roadmap

Each milestone remains incomplete until all acceptance criteria are met.

## Milestone 0 — Board and toolchain definition

- Confirm the exact STM32F446RE board and schematic.
- Record the debugger, compiler, IDE/build system, and versions.
- Add the correct startup file, CMSIS device headers, linker script, and memory map.
- Produce a clean build and a repeatable debugging session.

**Acceptance:** a documented build can be reproduced and execution reaches `main()` under the debugger.

## Milestone 1 — Clock and GPIO bring-up

- Document reset clock assumptions.
- Configure one GPIO output using registers.
- Verify output timing electrically or with a debugger-assisted test.

**Acceptance:** source, build log, wiring/pin record, test steps, and observed result are committed.

## Milestone 2 — UART and reusable utilities

- Implement polling UART TX/RX.
- Add timeouts and error reporting.
- Add an interrupt-driven RX path and circular buffer.
- Create a basic command/status interface.

**Acceptance:** transmitted and received data are verified, including overflow and timeout behaviour.

## Milestone 3 — SPI and W25Q flash

- Verify SPI signalling with a loopback or controlled slave test.
- Read and validate the W25Q JEDEC ID.
- Implement read, page-program, erase, busy polling, and bounds checks.
- Document flash endurance and page/sector constraints from the selected datasheet.

**Acceptance:** known data patterns survive write, readback, erase, boundary, and power-cycle tests.

## Milestone 4 — I2C, ADC, timer/PWM, and CAN

- Add one driver at a time.
- Create a focused example and failure test for each peripheral.
- Keep board-specific pin and clock configuration separate from reusable driver logic.

**Acceptance:** every driver has documented hardware setup, repeatable tests, and known limitations.

## Milestone 5 — FreeRTOS foundation

- Integrate FreeRTOS with documented configuration.
- Create acquisition, logger, command, and health-monitor tasks.
- Use queues or task notifications for data flow.
- Add a mutex only where shared-resource ownership requires it.
- Record task priorities, timing assumptions, and measured stack high-water marks.

**Acceptance:** task communication and timing are demonstrated without race conditions or unexplained failures.

## Milestone 6 — Circular data logger

- Define a versioned record format with length and integrity fields.
- Implement RAM buffering and circular flash addressing.
- Handle wrap-around, invalid records, full storage, and interrupted writes.
- Add UART commands to inspect or export records.

**Acceptance:** long-duration logging, wrap-around, reset recovery, and corrupted-record handling are tested.

## Milestone 7 — Reliability and documentation

- Add watchdog servicing based on system health rather than a single task.
- Add fault injection and boundary tests.
- Add build/test automation where practical.
- Publish a final architecture diagram, test report, and demonstration instructions.

**Acceptance:** another developer can reproduce the build and tests from the repository documentation.
