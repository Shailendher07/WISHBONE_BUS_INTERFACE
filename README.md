# Wishbone Bus Interface – SystemVerilog Design & Verification

🔗 **EDA Playground Simulation Link:**  
https://edaplayground.com/x/ZHSu  

---

## Project Overview
This project implements a Wishbone point-to-point bus interface for on-chip communication between Master and Slave IP cores in SoC designs.

The design supports:
- Single-cycle read/write transfers  
- Block/Burst transfers using CTI signaling  

Scalability verified across multiple data widths:
- 8-bit, 16-bit, 32-bit, and 64-bit

Core Wishbone signals (`cyc`, `stb`, `ack`, `we`, `adr`, `dat`, `cti`) are thoroughly verified through simulation to ensure proper timing, sequencing, and protocol adherence.

---

## Objectives
- Implement Wishbone Master and Slave with correct handshake logic  
- Support single & burst data transactions  
- Ensure valid address & data sequencing during burst operations  
- Develop a SystemVerilog testbench ensuring complete protocol compliance  

---

## Master Design
The Master module follows an FSM-based architecture with:
- **IDLE**, **BUS_REQUEST**, **BUS_WAIT** states  

It generates key control signals:
- `cyc_o`, `stb_o`, `we_o`, `sel_o`, `adr_o`, `dat_o`, `cti_o`

Features:
- Handles both classic and burst transfers  
- Terminates burst with **CTI = 3'b111**  
- Captures Slave responses (`ack_i`, `err_i`)  

---

## Slave Design
A synchronous memory-based Wishbone Slave supporting:
- Read, Write, Burst access modes  

### CTI Mode Support
| CTI Code | Operation Type |
|---------|----------------|
| `000` | Classic single transfer |
| `001` | Incrementing burst |
| `010` | Linear burst |
| `111` | Burst end |

Additional Features:
- Burst counter to maintain sequencing  
- Generates **ACK** and **ERR** responses  
- **Tag-Add** feature: returns `mem[0] + mem[1]`  

---

## Top-Level Integration
- Connects Master and Slave together for functional Wishbone operation  
- Ensures proper handshake and CTI decoding  
- Includes debug (`dbg_*`) signals to aid waveform monitoring  

---

## Verification Environment
A modular SystemVerilog testbench with:
- **Generator** → Constrained-random stimulus  
- **Driver** → Drives protocol-valid signal sequences  
- **Monitor** → Observes DUT behavior  
- **Scoreboard** → Memory integrity checking against expected reference  

Verification ensures:
- Correct handshake & protocol sequencing  
- Accurate CTI-based burst execution  
- Wait-state handling  
- Valid ACK/ERR signaling  
- Tag-Add functional correctness  
- Burst completion with `CTI = 3'b111`  

---

## Test Scenarios
- Classic read/write cycles  
- Incrementing & linear burst sequences  
- Burst termination with CTI = 111  
- ERR signaling for invalid address access  

---

## Simulation Results
Waveform & log-based functional validation confirm:
- Correct Wishbone timing & state transitions  
- Data integrity maintained in single & burst transactions  
- Protocol compliance under all tested conditions  
