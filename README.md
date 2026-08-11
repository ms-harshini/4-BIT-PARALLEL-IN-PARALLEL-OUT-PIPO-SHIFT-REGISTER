# 4-Bit PIPO Shift Register using Verilog

## 📌 Project Overview

This project implements a **4-Bit Parallel-In Parallel-Out (PIPO) Shift Register** using Verilog HDL.

A PIPO shift register accepts all bits of data simultaneously through parallel inputs and transfers the complete data word simultaneously to the parallel outputs.

---

## 🎯 Objective

The objective of this project is to design and verify a 4-bit PIPO shift register using Verilog HDL.

The project demonstrates:

* Parallel data input
* Parallel data output
* Synchronous data transfer
* Sequential logic
* Flip-flop based storage
* Reset operation
* Clocked data transfer
* Waveform generation
* Functional verification

---

## 🛠️ Technologies Used

* Verilog HDL
* VS Code
* Icarus Verilog
* GTKWave
* ModelSim / QuestaSim
* GitHub

---

## 📂 Project Structure

```text
4-Bit-PIPO-Shift-Register/
│
├── README.md
├── src/
│   └── pipo_shift_register.v
├── testbench/
│   └── tb_pipo_shift_register.v
├── simulation/
│   └── simulation_results.txt
└── waveform/
    └── waveform.vcd
```

---

## 🔢 Inputs and Outputs

| Signal       | Width | Description          |
| ------------ | ----: | -------------------- |
| CLK          | 1-bit | Clock signal         |
| RESET        | 1-bit | Clears the register  |
| PARALLEL_IN  | 4-bit | Parallel data input  |
| PARALLEL_OUT | 4-bit | Parallel data output |

---

## 🧠 What is PIPO?

PIPO stands for:

**Parallel-In Parallel-Out**

All bits are supplied simultaneously to the register.

For a 4-bit register:

```text
PARALLEL_IN = 1010
```

After the rising clock edge:

```text
PARALLEL_OUT = 1010
```

All four bits are transferred together.

---

## ⚙️ Working Principle

The register loads the complete 4-bit input on every rising edge of the clock.

The main operation is:

```verilog
parallel_out <= parallel_in;
```

Therefore:

```text
Input:
1010

Clock ↑

Output:
1010
```

---

## 🔄 Example Operation

Suppose:

```text
PARALLEL_IN = 1010
```

After the rising edge:

```text
PARALLEL_OUT = 1010
```

Next:

```text
PARALLEL_IN = 1100
```

After the next rising edge:

```text
PARALLEL_OUT = 1100
```

The complete data word is transferred simultaneously.

---

## ⏱️ Clock Operation

The data transfer occurs on the rising edge of the clock:

```verilog
always @(posedge clk or posedge reset)
```

Therefore, changes in `PARALLEL_IN` are captured when the clock rises.

---

## 🔌 Reset Operation

When:

```text
RESET = 1
```

the output is cleared:

```text
PARALLEL_OUT = 0000
```

The reset is asynchronous.

---

## 📈 Waveform Generation

The testbench automatically creates:

```text
waveform/waveform.vcd
```

The waveform contains:

```text
clk
reset
parallel_in
parallel_out
```

The waveform demonstrates the transfer of each 4-bit input value to the output.

---

## ▶️ Simulation Using Icarus Verilog

Open the VS Code terminal inside the project directory.

### Compile

```bash
iverilog -o pipo_sim src/pipo_shift_register.v testbench/tb_pipo_shift_register.v
```

### Run

```bash
vvp pipo_sim
```

The simulation generates:

```text
waveform/waveform.vcd
```

---

## 📊 Open the Waveform

Use GTKWave:

```bash
gtkwave waveform/waveform.vcd
```

Add these signals:

```text
clk
reset
parallel_in
parallel_out
```

You should observe the parallel input being transferred to the parallel output at each rising clock edge.

---

## 🖼️ Waveform Screenshot

After opening the waveform in GTKWave, save a screenshot as:

```text
waveform/waveform.png
```

Then add:

```markdown
## 📊 Simulation Waveform

![4-Bit PIPO Shift Register Waveform](waveform/waveform.png)
```

---

## 🧪 Testbench Verification

The testbench verifies:

* Reset operation
* Parallel loading
* Parallel output
* Multiple input patterns
* Clocked data transfer
* Waveform generation

Test patterns include:

```text
1010
1100
0011
1111
0101
```

---

## 📚 Concepts Demonstrated

* PIPO shift registers
* Parallel data transfer
* Sequential logic
* Flip-flops
* Data storage
* Clocked circuits
* Reset logic
* Testbench development
* VCD waveform generation
* Functional verification

---

## 🚀 Applications

PIPO registers are used in:

* Temporary data storage
* Digital systems
* CPU registers
* Data buffering
* FPGA designs
* Digital signal processing
* Microprocessor systems
* Data transfer circuits

---

## 🔗 Block Diagram

```text
              4-BIT PIPO REGISTER

PARALLEL_IN
   │ │ │ │
   ▼ ▼ ▼ ▼
┌───────────────┐
│               │
│  4-BIT        │
│  REGISTER     │
│               │
└───────┬───────┘
        │ │ │ │
        ▼ ▼ ▼ ▼
    PARALLEL_OUT
```

---

## 🔄 Shift Register Comparison

| Register | Input    | Output   |
| -------- | -------- | -------- |
| SISO     | Serial   | Serial   |
| SIPO     | Serial   | Parallel |
| PISO     | Parallel | Serial   |
| PIPO     | Parallel | Parallel |

---

## 🚀 Future Improvements

This project can be extended to:

* 8-bit PIPO register
* 16-bit PIPO register
* PIPO with enable
* Universal shift register
* Register bank
* FPGA implementation
* CPU register implementation

---

## 👩‍💻 Author

**Harshu**

B.Tech - Electronics and Communication Engineering

---

## 📄 License

This project is created for educational and academic purposes.
```verilog
`timescale 1ns/1ps

module pipo_shift_register (
    input  wire       clk,
    input  wire       reset,
    input  wire [3:0] parallel_in,
    output reg  [3:0] parallel_out
);

    always @(posedge clk or posedge reset) begin

        if (reset) begin
            parallel_out <= 4'b0000;
        end

        else begin
            parallel_out <= parallel_in;
        end

    end

endmodule
```
```verilog
`timescale 1ns/1ps

module tb_pipo_shift_register;

    reg clk;
    reg reset;
    reg [3:0] parallel_in;

    wire [3:0] parallel_out;

    pipo_shift_register DUT (
        .clk(clk),
        .reset(reset),
        .parallel_in(parallel_in),
        .parallel_out(parallel_out)
    );

    // Clock generation
    always #5 clk = ~clk;

    // Waveform generation
    initial begin
        $dumpfile("waveform/waveform.vcd");
        $dumpvars(0, tb_pipo_shift_register);
    end

    initial begin

        clk = 1'b0;
        reset = 1'b1;
        parallel_in = 4'b0000;

        $display("================================================");
        $display("        4-BIT PIPO SHIFT REGISTER TEST");
        $display("================================================");
        $display("TIME | RESET | PARALLEL_IN | PARALLEL_OUT");
        $display("-----------------------------------------------");

        // Reset
        #10;

        reset = 1'b0;

        // Test 1
        parallel_in = 4'b1010;

        @(posedge clk);
        #1;

        $display(
            "%0t |   %b   |     %b      |     %b",
            $time,
            reset,
            parallel_in,
            parallel_out
        );

        // Test 2
        parallel_in = 4'b1100;

        @(posedge clk);
        #1;

        $display(
            "%0t |   %b   |     %b      |     %b",
            $time,
            reset,
            parallel_in,
            parallel_out
        );

        // Test 3
        parallel_in = 4'b0011;

        @(posedge clk);
        #1;

        $display(
            "%0t |   %b   |     %b      |     %b",
            $time,
            reset,
            parallel_in,
            parallel_out
        );

        // Test 4
        parallel_in = 4'b1111;

        @(posedge clk);
        #1;

        $display(
            "%0t |   %b   |     %b      |     %b",
            $time,
            reset,
            parallel_in,
            parallel_out
        );

        // Test 5
        parallel_in = 4'b0101;

        @(posedge clk);
        #1;

        $display(
            "%0t |   %b   |     %b      |     %b",
            $time,
            reset,
            parallel_in,
            parallel_out
        );

        $display("================================================");
        $display("Simulation completed successfully.");
        $display("================================================");

        $finish;

    end

endmodule
```
# 4-BIT PIPO SHIFT REGISTER SIMULATION RESULTS

RESET:
PARALLEL_OUT = 0000

TEST 1:
PARALLEL_IN  = 1010
PARALLEL_OUT = 1010

TEST 2:
PARALLEL_IN  = 1100
PARALLEL_OUT = 1100

TEST 3:
PARALLEL_IN  = 0011
PARALLEL_OUT = 0011

TEST 4:
PARALLEL_IN  = 1111
PARALLEL_OUT = 1111

TEST 5:
PARALLEL_IN  = 0101
PARALLEL_OUT = 0101

The complete 4-bit input is loaded simultaneously
on every rising edge of the clock.

================================================
Simulation completed successfully.
==================================
