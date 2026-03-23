# Conveyor Control System (STM32 + CAN + C)

## Overview

This project explores the design of a simple embedded control system for a conveyor using a finite state machine and CAN-based communication.

The system is structured around clear control logic, separating high-level command input from low-level execution. It is intended as a learning and demonstration project for embedded systems concepts, particularly in the context of industrial automation, and is being developed on STM32 hardware.

---

## Hardware Platform

- STM32F769I-DISCO development board (Cortex-M7)
- Embedded peripherals (timers, GPIO, UART)
- Planned CAN integration (external transceiver required)

> The STM32 acts as the controller. External motor driver or power electronics are required to physically drive a conveyor.

---

## System Architecture

The system is divided into two layers:

* **Linux Controller (High-Level)**

  * Sends start/stop commands via CAN  
  * Represents external or supervisory control  

* **Embedded Controller (STM32)**

  * Executes control logic  
  * Manages state transitions  
  * Interfaces with hardware and control outputs  

---

## State Machine

The core of the system is a finite state machine with three states:

* **IDLE** — system is inactive and waiting for a command  
* **RUNNING** — conveyor is operating  
* **STOPPED** — system is halted (e.g. due to stop command or fault)  

### Transitions

* IDLE → RUNNING: triggered by a start command (if no faults)  
* RUNNING → STOPPED: triggered by a stop command or fault  
* STOPPED → RUNNING: triggered by a start command (if no faults)  

Transitions are deterministic and event-driven.

---

## Control Modes

The system supports two conceptual control approaches:

### 1. Command-Based Control (Primary)

* Driven by commands from the Linux controller (via CAN)  
* Determines when the system starts and stops  
* Represents real-world supervisory control  

### 2. Time-Based Control (Secondary)

* Uses internal timing to cycle between states  
* Example: run for a fixed duration, then stop, repeat  
* Useful for testing or simple automation scenarios  

> Command-based control is the primary mode of operation. Time-based control is optional.

---

## Communication (CAN)

Control commands are transmitted over CAN.

Typical examples:

* START command → transition to RUNNING  
* STOP command → transition to STOPPED  

The CAN interface links the Linux controller and the STM32.

---

## Execution Model

The system follows a simple loop:

1. Read inputs (CAN messages, timers)  
2. Update state  
3. Execute outputs  
4. Emit trace/log output  

sense → decide → act → repeat  

---

## Logging / Trace

The system includes a simple trace mechanism for visibility and debugging.

Two types of logging are used:

* **Time-based logging** — periodic system status  
* **Event-based logging** — state transitions and key events  

### Example

```
[0001.000] STATE=RUNNING
[0001.100] EVENT=TICK
[0002.000] STATE=STOPPED
```

---

## Design Goals

* Clear and deterministic state transitions  
* Separation of control (Linux) and execution (STM32)  
* Real hardware implementation of embedded logic  
* Focus on real-world embedded patterns (CAN, state machines)  

---

## Notes

This repository focuses on system design and learning.  
Implementation code is kept local.

---

## Future Work

* Full CAN message specification (IDs, payloads)  
* CAN transceiver integration and testing  
* Motor driver / actuator interface  
* Integration with SocketCAN (Linux)  
* Hardware testing on STM32  
* Fault handling and safety extensions  
* Expanded logging and diagnostics  

---

## Summary

This project demonstrates a structured approach to embedded control using:

* Finite state machines  
* CAN-based communication  
* STM32 hardware execution  
* Event-driven design  

It serves as a foundation for more advanced embedded and industrial control systems.

---

## Coming Soon

* State machine design notes  
* CAN communication basics  
* Pointers and memory understanding  
* Embedded system debugging  
