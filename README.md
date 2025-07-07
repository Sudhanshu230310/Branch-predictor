# Branch Predictor Simulator in C++

This project parses a CPU instruction trace (RISC‑V style) and simulates multiple branch predictors, including static and dynamic schemes, while building a simple Branch Target Buffer (BTB). It reports accuracy metrics and dumps prediction tables to text files.

---

## Features

* **Trace Parsing**: Reads an `input.txt` trace containing lines like `core 0: 0xADDRESS (0xMACHINE_CODE)`.
* **Instruction Decoding**: Identifies branch instructions (`beq`, `bne`, `blt`, etc.), as well as `JAL` and `JALR`.
* **Static Predictors**:

  * **Always Taken**
  * **Always Not Taken**
* **Dynamic Predictors**:

  * **1-Bit Predictor** (single-bit saturating)
  * **2-Bit Predictor** (two-bit saturating)
* **Branch Target Buffer**:

  * Records `<branch address, target address>` entries for each branch.
  * Outputs `Branch Target Buffer.txt`.
* **Outcome Recording**:

  * Captures original branch outcomes (`T`/`N`) in `Original_outcomes.txt`.
  * Dumps per-address history in `Branch History Table.txt`.
* **Accuracy Metrics**:

  * Prints overall percentage accuracy for each predictor.

---

## Prerequisites

* C++ compiler with C++11 support (e.g., `g++`, `clang++`).

---

## Building

```bash
# Compile the simulator
g++ -std=c++11 main.cpp -o branch_predictor
```

---

## Input Format (`input.txt`)

Each line representing a fetch from `core 0` should look like:

```
core 0: 0x<PC> (<0xMACHINE_CODE>)
```

* **PC**: Program counter in hexadecimal.
* **MACHINE\_CODE**: 32‑bit instruction in hexadecimal.

Lines not matching this pattern are ignored.

---

## Running

```bash
# Ensure input.txt is in the working directory
./branch_predictor
```

* **Console Output**:

  * Accuracy for `ALWAYS_TAKEN` and `ALWAYS_NOT_TAKEN` predictors.
  * Accuracy for 1‑bit and 2‑bit dynamic predictors.
* **Generated Files**:

  * `Branch Target Buffer.txt`: BTB contents.
  * `Original_outcomes.txt`: Sequence of actual branch outcomes.
  * `Branch History Table.txt`: Per-branch-address outcome history.

---

## Code Structure

* **Hex/Bin Conversion**: `hexStringToBinary`, `binaryToHexadecimal`, `hexToDecimal`, `inttohex`.
* **Decoding**: `decode()` classifies instructions and counts branches.
* **Immediate Calculation**: `immediate()` extracts branch offsets.
* **Prediction Schemes**:

  * `always_taken()`, `always_not_taken()`
  * `one_bit_dynamic()`, `two_bit_dynamic()`
* **BTB Management**:

  * `display_BTB()` writes `<PC, target>` pairs to file.
* **Main Flow**:

  1. Parse trace, collect PC and machine codes.
  2. Decode instructions and determine taken/not‑taken.
  3. Populate BTB and branch outcome lists.
  4. Run each predictor and print accuracy.
  5. Write output tables to files.

---

## Limitations & Future Work

* **Configurable Trace File**: Allow custom input filenames via CLI.
* **N‑Bit Predictors**: Extend to 4‑bit or tournament predictors.
* **BTB Capacity**: Add size limits and replacement policies.
* **Memory Cleanup**: Use RAII or smart pointers for dynamic data.
* **Parallel Simulation**: Speed up processing for large traces.

---

## License

Distributed under the **MIT License**. Feel free to use, modify, and distribute.
