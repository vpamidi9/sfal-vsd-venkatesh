
# SFAL-VSD-SoC Design and Implementation

I am excited to share the details of my 12-week SoC Design and Implementation course. The course kicks off with 6 weeks dedicated to foundational semiconductor design concepts, covering essentials such as floorplanning, placement, CTS, routing, optimization, and verification. During this time, I will gain comprehensive knowledge ranging from chip size estimation to signal integrity solutions.

Following the initial phase, I have the opportunity to select a specialization topic. This allows me to delve deeper into an area of interest, guided by industry experts, ensuring targeted and in-depth learning.

The course then transitions to a 4-week segment focused on advanced, industry-grade concepts. These sessions are led by professionals and culminate in a challenging design project that simulates real-world scenarios, providing practical, hands-on experience.

In the final 2 weeks, the emphasis shifts to professional skills development. This includes mastering GitHub documentation and navigating internship selection processes, essential for a successful career in the semiconductor industry.

This program offers a perfect blend of foundational learning, specialization, and practical application. It is designed to prepare me for a successful career in SoC design and implementation, balancing theoretical knowledge with hands-on experience. Through this program, I will:

- Master the entire SoC design process, from floorplanning to physical verification.
- Personalize my learning by selecting a specialization topic.
- Gain valuable insights from industry experts through an in-depth design project.
- Enhance my professional portfolio with GitHub documentation skills.
- Increase my chances of securing internships in the semiconductor industry.

This is a unique opportunity to transform my passion into expertise and advance my career in SoC design and implementation!

## Day To Day Progress

<details>
    <summary>🛠️ Day 0 - System Set Up and Tools Installation</summary>
    
## System Set Up and Tools Installation

**Instructions on how to install tools:**

Download the Oracle Virtual Machine - [VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)

**System Configuration:**

- 6GB RAM
- 50GB HDD
- Ubuntu 20.04+
- 4vCPU

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/bde8d833-abdd-4a08-bbc3-fcc341b97227)

**Tool Installation:**

**1. Yosys**

```sh
$ sudo apt-get update
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ sudo apt install make (If make is not installed please install it)
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make config-gcc
$ make
$ sudo make install
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/136cb330-2c9a-4d3b-93d0-8ec554c1f742)

**2. iverilog**

```sh
$sudo apt-get update
$sudo apt-get install iverilog
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/e9585bf3-4f92-4dbb-a18a-7d018e64467a)

**3. GTKwave**

```sh
$sudo apt-get update
$sudo apt install gtkwave
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/5d789652-3560-4a5a-8c36-1c9556fea5be)

</details>

<details>
    <summary>🛠️ Day 1 - Introduction to Verilog RTL Design and Synthesis</summary>

## Introduction to Verilog RTL Design and Synthesis

### Introduction to iVerilog OpenSource Simulator

**iverilog**: Icarus Verilog, commonly known as Iverilog, is an open-source tool used for the simulation and synthesis of digital circuits described in Verilog hardware description language (HDL).

Primarily, Iverilog is used to simulate Verilog designs, allowing designers to verify the functionality of their digital circuits before physical implementation.

Let's take `good_mux.v` design as an example:

```verilog
module good_mux (input i0 , input i1 , input sel , output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

Let's see its Testbench `tb_good_mux.v`:

```verilog
`timescale 1ns / 1ps
module tb_good_mux;
// Inputs
reg i0,i1,sel;
// Outputs
wire y;

// Instantiate the Unit Under Test (UUT)
good_mux uut (
    .sel(sel),
    .i0(i0),
    .i1(i1),
    .y(y)
);

initial begin
    $dumpfile("tb_good_mux.vcd");
    $dumpvars(0,tb_good_mux);
    // Initialize Inputs
    sel = 0;
    i0 = 0;
    i1 = 0;
    #300 $finish;
end

always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule
```

**Simulation of good_mux:**

```sh
$ iverilog good_mux.v tb_good_mux.v
```

The output will be **a.out** which specifies the output file for the compiled simulation.

```sh
$ ./a.out
```

This command runs the simulation and generates a waveform dump file (in this case, `tb_good_mux.vcd`) as specified in the testbench.

To view the simulation results, use a waveform viewer like GTKWave. Open GTKWave and load the generated VCD file:

```sh
$ gtkwave tb_good_mux.vcd
```

GTKWave will open, allowing you to inspect the signals and verify the behavior of your design.

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d19c2602-6247-4ba2-8980-92dd27f52394)

### Introduction to Yosys

Yosys is an open-source framework for Verilog RTL synthesis. It is used primarily for converting high-level Verilog descriptions of digital circuits into gate-level netlists that can be implemented on FPGAs or used for ASIC design. Yosys is highly versatile and supports various front-end and back-end tools, making it a valuable tool for digital design and synthesis.

**Inputs for Yosys tool:** Design (.v), Liberty (.lib)  
**Output:** Netlist file (.net.v)

### Synthesis of a good_mux by opening Yosys:

**Read the Liberty source file:**

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/615c393f-ca85-4d53-8ef2-0df77da89bed)

**Read the Verilog source file:**

```sh
yosys> read_verilog good_mux.v
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/fd82b4ae-3614-407b-9ded-5969d212e413)

**Perform synthesis:**

```sh
yosys> synth -top good_mux
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/49a325bf-c11c-4160-ba3c-540cc42b8066)

**Technology Mapping to the Design using the `abc` tool which is integrated with Yosys:**

```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/1d3345aa-668b-4753-bc33-f26b5c47cd89)

**Write the synthesized netlist to a Verilog file:**

```sh
yosys> write_verilog -noattr good_mux.net.v
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d1506467-d9fe-4513-9bfc-108c4c73375b)

**View the generated gate level netlist:**

```sh
yosys> show
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/8909cc50-ba0f-4ed7-b2b3-11782656e566)

</details>

<details>
	<summary>🛠️Day 2 - Timing libs, Hierarchical vs Flat Synthesis and Efficient Flop Coding Styles </summary>

## Timing libs, Hierarchical vs Flat Synthesis and Efficient Flop Coding Styles

## Introduction to timing.lib

A .lib file, also known as a Liberty file, is a standard format used in the electronic design automation (EDA) industry to describe the timing, power, and area characteristics of the standard cells in a digital library.

**Key Contents of a .lib File:** Timing information, Power information, Area information, Operating conditions, Pin descriptions.

In this lab, we use **sky130_fd_sc_hd_tt_025C_1v80.lib**. Here's a breakdown of the filename and what each part signifies:

- **sky130:** Refers to the 130nm technology node provided by SkyWater Technology Foundry.
- **fd:** Stands for fully-depleted, indicating the type of process technology.
- **sc:** Stands for standard cell.
- **hd:** Stands for high-density standard cell library.
- **tt:** Typical process corner (typical-typical).
- **025C:** The temperature condition at which the library data is characterized (25°C).
- **1v80:** The operating voltage condition (1.8V).

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c8748f93-e472-492c-bdf3-a71b3a26eeb5)

## Hierarchical vs Flat Synthesis

**Hierarchical synthesis:** Hierarchical synthesis involves organizing the design into a hierarchy of modules or blocks, where each module represents a functional unit or a logical partition of the design.

**Read the Liberty source file:**

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/615c393f-ca85-4d53-8ef2-0df77da89bed)

**Read the Verilog source file:**

```sh
yosys> read_verilog multiple_modules.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/48d780ce-5c8b-4c4b-aa63-fc87c373b0d0)

**Perform synthesis:**

```sh
yosys> synth -top multiple_modules
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/b5fdec51-ceaa-4de6-a425-5d5ca0c20d7a)

**Technology Mapping to the Design using *abc* tool which is integrated with Yosys:**

```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/27ce5ad2-5528-4b44-b466-6a4f7bac390a)

**View the generated gate level netlist:**

```sh
yosys> show multiple_modules
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/dc5ef610-0836-424d-93a4-9506f576a33e)

**Flat synthesis:** Flat synthesis involves synthesizing the entire design as a single, monolithic entity without hierarchical organization.

**To flatten the design:**

```sh
yosys> flatten
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/41ea4f42-dfd8-4cfd-9b0a-215cec6f2a3c)

**To write a netlist to .v file:**

```sh
yosys> write_verilog -noattr multiple_modules_flat.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/a33036ba-4305-48db-a714-d438269da691)

**To view the netlist (.v):**

```sh
yosys> !givim multiple_modules_flat.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/04d6e3f8-2307-452b-87f4-0ff5cbab5025)

**To view the flattened gate level netlist:**

```sh
yosys> show
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/1d37fa5f-6bbf-402e-94d7-5232a2348038)

## Sub-Module level synthesis

**Read the Liberty source file:**

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/615c393f-ca85-4d53-8ef2-0df77da89bed)

**Read the Verilog source file:**

```sh
yosys> read_verilog multiple_modules.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/48d780ce-5c8b-4c4b-aa63-fc87c373b0d0)

**Perform synthesis:**

```sh
yosys> synth -top sub_module1
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/10a7fcae-1eea-4453-88cf-12d8ff4fbe21)

**Technology Mapping to the Design using *abc* tool which is integrated with Yosys:**

```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/2393c805-815c-45af-9046-187d3648a313)

**To view the generated gate level netlist:**

```sh
yosys> show 
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/eee9dda1-5adb-46db-9f2b-f8a7c5203adc)

## Different Methods for Flip-Flop Coding and Performance Enhancement

**Why are flops necessary, and how do they mitigate glitches in the circuit?**

Glitches can manifest in digital circuits due to factors like signal propagation delays, noise interference, or timing discrepancies. Flops play a crucial role in preventing glitches during circuit operation through the following mechanisms:

- **Synchronization:** Flops operate as edge-triggered components, responding exclusively to transitions in the input signal, such as rising or falling edges. This synchronization mechanism ensures that the output changes occur only at specific moments, minimizing the potential for glitches induced by transient signal fluctuations.
- **Timing Control:** Flops are typically governed by a clock signal, ensuring that all circuit activities unfold synchronously. This synchronized operation eliminates timing discrepancies that could otherwise lead to glitches stemming from data arriving at disparate times.

**Flop coding styles:**

- **Simulation of Asynchronous Reset D-Flip Flop using iverilog followed by GTKWave**

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/4398cdb4-a0ef-489b-a7cd-17b14378227a)

- **Simulation of Asynchronous Set D-Flip Flop using iverilog followed by GTKWave**

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/485cab5e-6c70-4176-9d5f-a4de986bf69d)

- **Simulation of Synchronous Reset D-Flip Flop using iverilog followed by GTKWave**

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/7d1ced01-3428-4308-8d2f-ad07fe6d6afb)

**Synthesis of Asynchronous Reset D-Flip Flop using yosys:**

  Here, we have to map the Flip-Flops to the *dfflib* which is present in *sky130_fd_sc_hd_tt_025C_1v80.lib*

  Here's the command for mapping the flipflops to the dfflib:

  ```sh
  yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
  ```
  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c784d898-40e0-4c8e-a2eb-ca1a7c55e61f)

-

 **Synthesis of Asynchronous set D-Flip Flop using yosys:**

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/5ac9f787-30ca-4d22-860c-5a163fd10d40)

- **Synthesis of Synchronous Reset D-Flip Flop using yosys:**

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c44d9314-3979-490e-9a33-131d8494b550)

</details>

<details>
    <summary>🛠️Day 3- Combinational and Sequential Optimization</summary>
    <ul>
        <li>
            <details>
                <summary>Combinational Logic Optimization</summary>
                <ul>
                    <li>
                        <details>
                            <summary><strong>PART 1: For opt_check Modules</strong></summary>
                            <ol>
                                <li>
                                    <strong>Step 1: Read Library</strong>
                                    <p>In Yosys, execute the command to read the library:</p>
                                    <img width="728" alt="Read Library" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/bf2a8b14-da19-41ff-96ac-ee1fc0572722">
                                </li>
                                <li>
                                    <strong>Step 2: Read Verilog File</strong>
                                    <p>Load the Verilog file for the 'opt_check' module:</p>
                                    <img width="652" alt="Verilog File" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/1c68e3e0-f349-4a91-9aa0-df769f531e71">
                                </li>
                                <li>
                                    <strong>Step 3: Define Module for Synthesis</strong>
                                    <p>Define the module to be synthesized and view the number of cells in the module:</p>
                                    <img width="286" alt="Define Module" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/7e9cb74a-e0d5-4779-b786-eb7d62618c60">
                                    <img width="420" alt="Cell Count" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/ea4b5d4a-122a-4881-903f-66d5a37996c0">
                                </li>
                                <li>
                                    <strong>Step 4: Execute opt_clean</strong>
                                    <p>Run opt_clean to remove unused cells and wires:</p>
                                    <img width="623" alt="opt_clean Execution" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/b9d682ef-5229-4092-9dff-ac44c408aff0">
                                </li>
                                <li>
                                    <strong>Step 5: Generate Netlist</strong>
                                    <p>Generate the netlist and observe the reduction in the number of cells:</p>
                                    <img width="611" alt="Netlist Generation" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4d887668-b080-4942-a05d-5350f1ac6e51">
                                    <img width="598" alt="Cell Reduction" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/78dbf1ca-d2c6-458c-9b3e-8ef595bd0c82">
                                </li>
                                <li>
                                    <strong>Step 6: View Netlist Design</strong>
                                    <p>Execute the show command to view the netlist design:</p>
                                    <img width="611" alt="View Netlist" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/0992c28f-063b-4a43-90b0-b2abdfad762b">
                                </li>
                                <li>
                                    <strong>Steps 7-12: Repeat for Additional Modules</strong>
                                    <p>Repeat the above steps for additional modules (opt_check2, opt_check3, opt_check4), observing the changes and improvements each time:</p>
                                    <img width="418" alt="Further Steps" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/16b97a65-65b6-4b51-ac38-3218c9d2865d">
                                    <img width="556" alt="ABC Command" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/17c9dd9f-fd22-4312-bb51-0be49eadb040">
                                    <img width="497" alt="Further ABC Command" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/3607a834-f446-462b-ab11-25856870721a">
                                </li>
                            </ol>
                        </details>
                    </li>
                    <li>
                        <details>
                            <summary><strong>PART 2: multiple_modules Optimization</strong></summary>
                            <ol>
                                <li>
                                    <strong>Step 1: Read Verilog File</strong>
                                    <p>Load the Verilog file for 'multiple_modules_opt.v'.</p>
                                    <img width="744" alt="Read Verilog File" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4ef7a599-3701-4953-8c1a-450a923a9876">
                                </li>
                                <li>
                                    <strong>Step 2: Define the Module for Synthesis</strong>
                                    <p>Specify which module to synthesize.</p>
                                    <img width="398" alt="Define Module" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/73c1c313-ae77-45ad-9814-436b4d1cdebf">
                                </li>
                                <li>
                                    <strong>Step 3: Flatten the Design</strong>
                                    <p>Apply design flattening techniques to simplify the hierarchy.</p>
                                    <img width="475" alt="Flatten Design" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/7dd4686a-7897-4ba8-8a65-52111a04903d">
                                </li>
                                <li>
                                    <strong>Step 4: Execute opt_clean</strong>
                                    <p>Remove unused cells and wires to optimize the design.</p>
                                    <img width="635" alt="Execute opt_clean" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4bfecaba-c49b-4ca9-958e-f1e7486aaa62">
                                </li>
                                <li>
                                    <strong>Step 5: Generate the Netlist</strong>
                                    <p>Generate the netlist and note the reduction in the number of cells.</p>
                                    <img width="617" alt="Generate Netlist" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/c7b85d9d-f18d-4992-bf0b-151f22301e3d">
                                    <img width="571" alt="Cell Reduction" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/cf863f0b-27aa-4781-8b36-0e6bd0f6275a">
                                </li>
                                <li>
                                    <strong>Step 6: View Netlist Design</strong>
                                    <p>Display the synthesized netlist design to verify correctness and optimization.</p>
                                    <img width="595" alt="View Netlist Design" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/83c65d81-da59-49a9-8735-3938a35d7c18">
                                </li>
                                <li>
                                    <strong>Step 7: Repeat Optimization for Additional Module</strong>
                                    <p>Repeat the optimization steps for 'multiple_modules_opt2.v' and observe changes.</p>
                                </li>
                                <li>
                                    <strong>Step 8: View Netlist Design for Additional Module</strong>
                                    <p>Review the final netlist design for 'multiple_modules_opt2.v'.</p>
                                    <img width="353" alt="Final Netlist Design" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/18374a6c-27ec-4bb3-9094-b057cbc6a9e2">
                                </li>
                            </ol>
                        </details>
                    </li>
                </ul>
            </details>
        </li>
        <li>
            <details>
                <summary>Sequential Logic Optimization</summary>
                <ul>
                    <li>
                        <details>
                            <summary><strong>PART 1: Dff_const Synthesis</strong></summary>
                            <ol>
                                <li>
                                    <strong>Step 1: Read the Library</strong>
                                    <p>Load the required library in Yosys.</p>
                                    <img width="735" alt="Read Library" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/c4b43fb9-4a90-4695-842f-d68680ce4f0b">
                                </li>
                                <li>
                                    <strong>Step 2: Read the Verilog File</strong>
                                    <p>Load the Verilog file for 'dff_const1.v'.</p>
                                    <img width="669" alt="Read Verilog File" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/e85367f2-5aa3-4692-9684-a5726016a08e">
                                </li>          
                                <li>
                                    <strong>Step 3: Define the Module for Synthesis</strong>
                                    <p>Specify the module to be synthesized and view the design hierarchy.</p>
                                    <img width="306" alt="Define Module" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/c5e25901-ce0f-4520-9e21-ac1d5bc2666c">
                                    <img width="422" alt="Design Hierarchy" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/80bc0503-0ade-47e4-bb10-a6773c53051f">
                                </li>
                                <li>
                                    <strong>Step 4: Run dfflibmap</strong>
                                    <p>Map the D flip-flop cells to sequential cells using dfflibmap.</p>
                                    <img width="870" alt="Run dfflibmap" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/76ce4935-130d-4692-a5b3-211c7211afdc">
                                </li>
                                <li>
                                    <strong>Step 5: Generate the Netlist</strong>
                                    <p>Create the netlist for the design.</p>
                                    <img width="611" alt="Generate Netlist" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4802d8a6-2763-4975-be62-805a4473ac2a">
                                </li>
                                <li>
                                    <strong>Step 6: View the Design</strong>
                                    <p>Execute the 'show' command to view the synthesized design.</p>
                                    <img width="594" alt="View Design" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/8dcc75b9-0bc2-43a3-89ae-9b0a1a8b6122">
                                </li>
                                <li>
                                    <strong>Steps 7-12: Repeat for Additional Files</strong>
                                    <p>Repeat the above steps for 'dff_const2.v', 'dff_const3.v', and 'dff_const4.v', viewing the design after each synthesis.</p>
                                    <img width="609" alt="View Design 2" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/481c7486-83c1-4fe3-8301-ba1f930bc791">
                                    <img width="1359" alt="View Design 3" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/5f8109ea-1f69-4d4f-bdb5-fb59e37cc882">
                                    <img width="616" alt="View Design 4" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/69a155b4-d899-4597-9631-59e497c7edb5">
                                </li>
                            </ol>
                        </details>
                    </li>
                    <li>
                        <details>
                            <summary><strong>PART 2: Sequential Optimizations for Unused Outputs</strong></summary>
                            <ol>
                                <li>
                                    <strong>Step 1: Read the Library</strong>
                                    <p>Load the required library in Yosys.</p>
                                    <img width="744" alt="Read Library" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/f2b3de6b-a2c1-498b-b957-aec0487c08be">
                                </li>
                                <li>
                                    <strong>Step 2: Read the Verilog File</strong>
                                    <p>Load the Verilog file for 'counter_opt.v'.</p>
                                    <img width="663" alt="Read Verilog File" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/c603c612-d903-4e05-ab1d-bde84a52fd3c">
                                </li>          
                                <li>
                                    <strong>Step 3: Define the Module for Synthesis</strong>
                                    <p>Specify the module to be synthesized and view the design hierarchy.</p>
                                    <img width="324" alt="Define Module" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/1a4979c3-0da7-4cf9-9bb2-a698f2b4c51d">
                                    <img width="407" alt="Design Hierarchy" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/ecd2ad69-1e69-44cf-8403-d8670bf7028e">
                                </li>
                                <li>
                                    <strong>Step 4: Run dfflibmap</strong>
                                    <p>Map the D flip-flop cells to sequential cells using dfflibmap.</p>
                                    <img width="873" alt="Run dfflibmap" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/65e34b53-4b7a-4d55-a43f-0cf59489a386">
                                </li>
                                <li>
                                    <strong>Step 5: Generate the Netlist</strong>
                                    <p>Create the netlist for the design.</p>
                                    <img width="621" alt="Generate Netlist" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/d4b49572-0224-4ca6-978d-71ace7464ce3">
                                </li>
                                <li>
                                    <strong>Step 6: View the Design</strong>
                                    <p>Execute the 'show' command to view the synthesized design.</p>
                                    <img width="1361" alt="View Design" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/26807584-5733-4728-bc9f-de30d5d18d4d">
                                </li>
                                <li>
                                    <strong>Steps 7-8: Repeat for Additional Files</strong>
                                    <p>Repeat the above steps for 'counter_opt2.v', viewing the design after synthesis.</p>
                                    <img width="425" alt="View Design 2" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/2fdaa020-ca5a-4bf7-9ca2-b4087914ac52">
                                    <img width="1370" alt="View Design 3" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/848db3bb-35bd-4fd2-8e92-122e76d86f70">
                                </li>
                            </ol>
                        </details>
                    </li>
                </ul>
            </details>
        </li>
    </ul>
</details>


<details>
    <summary>🛠️ Day 4 - Gate Level Simulation, Synthesis Simulation Mismatch, and Blocking & Non-Blocking Statements </summary>
    <ul>
        <li>
            <details>
                <summary>Lab on GLS and Synth Simulation Mismatch</summary>
                <ul>
                    <li>
                        <details>
                            <summary>PART 1: For ternary_operator_mux</summary>
                            <p>Step 1</p>
                            <pre>
Load ternary_operator_mux.v & its testbench to Iverilog.
<img width="1333" alt="Screenshot 2024-05-26 at 2 25 18 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/5fb664a3-428b-40b5-95c3-6e5e76385d2e">
                            </pre>
                            <p>Step 2</p>
                            <pre>
Execute a.out file.
<img width="831" alt="Screenshot 2024-05-26 at 2 25 28 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/0845279c-0fdb-458a-b911-593f7f138990">
                            </pre>
                            <p>Step 3</p>
                            <pre>
Load the .vcd file genrated into GTKWave.
<img width="895" alt="Screenshot 2024-05-26 at 2 25 46 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/a17efccf-4528-4b08-856c-e3366dea9441">
The ternary_operator_mux's behavior is analyzed on GTKWave                            
<img width="1374" alt="Screenshot 2024-05-26 at 2 26 56 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/b2f81f99-be77-4d8f-8c5f-c76efa0db555">
                            </pre>
                            <p>Step 4</p>
                            <pre>
Invoke Yosys by using command yosys
<img width="817" alt="Screenshot 2024-05-26 at 2 27 23 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/5aeace9b-91d1-43df-b7a7-95a22d2f7dca">
                            </pre>
                            <p>Step 5</p>
                            <pre>
Read the library using read_liberty
<img width="733" alt="Screenshot 2024-05-26 at 2 27 36 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/c7df753c-4859-4517-b10b-844be452fbc0">
                            </pre>
                            <p>Step 6</p>
                            <pre>
Read the ternary_operator_mux.v using read_verilog
<img width="754" alt="Screenshot 2024-05-26 at 2 27 54 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/9bd5d509-2035-4be9-b17f-084f80f58835">
                            </pre>
                            <p>Step 7</p>
                            <pre>
Define the module that needs to be synthesized
<img width="397" alt="Screenshot 2024-05-26 at 2 28 29 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/6b23f59f-4e02-43ec-932b-750d183b30f0">
                            </pre>
                            <p>Step 8</p>
                            <pre>
Generate the netlist using abc command
<img width="622" alt="Screenshot 2024-05-26 at 2 28 54 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/9565e3d5-66ba-45ef-8464-7cec13c5cdd4">
                            </pre>
                            <p>Step 9</p>
                            <pre>
Write the netlist to ternary_operator_mux_net.v
<img width="490" alt="Screenshot 2024-05-26 at 2 29 20 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/61065d52-6532-4a74-b8ae-34b8734fbce6">
                            </pre>
                            <p>Step 10</p>
                            <pre>
Execute show to view the design
<img width="609" alt="Screenshot 2024-05-26 at 2 29 30 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/76ee78c8-95fe-47e6-a9aa-ec2b6fa1e9c1">
                            </pre>
                            <p>Step 11</p>
                            <pre>
Exit yosys and load the ternary_operator_mux_net.v to iverilog.
<img width="1372" alt="Screenshot 2024-05-26 at 2 32 32 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/30cf677a-e529-4f09-a0e2-7d77f5ab48e5">
                            </pre>
                            <p>Step 12</p>
                            <pre>
Execute a.out file.
<img width="829" alt="Screenshot 2024-05-26 at 2 32 47 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/9194b986-d637-4919-9a9a-a66ef5fd8150">
                            </pre>
                            <p>Step 13</p>
                            <pre>
Load the generated .vcd file into GTKWave
<img width="1107" alt="Screenshot 2024-05-26 at 2 33 19 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4b04c1e3-5aff-4937-9670-706364e92fa3">
                            </pre>
                            <p>Step 14</p>
                            <pre>
Observe the GLS of ternary_operator_mux
<img width="1357" alt="Screenshot 2024-05-26 at 2 33 52 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/746f372e-4dd9-4da4-a4ff-57836f00945e">
                            </pre>
                        </details>
                    </li>
                    <li>
                        <details>
                            <summary>PART 2: For bad_mux</summary>
                            <p>Step 1</p>
                            <pre>
Load bad_mux.v & its testbench to Iverilog.
<img width="1072" alt="Screenshot 2024-05-26 at 2 47 47 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/3886f80b-b39e-4f91-9cfb-d7fd26f87a2b">
                            </pre>
                            <p>Step 2</p>
                            <pre>
Execute a.out file.
<img width="836" alt="Screenshot 2024-05-26 at 2 47 59 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/80bff7b8-6f38-42e9-b7ee-b527d9e06de7">
                            </pre>
                            <p>Step 3</p>
                            <pre>
Load the .vcd file genrated into GTKWave.
<img width="980" alt="Screenshot 2024-05-26 at 2 48 17 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/94c68039-9cd8-435f-b02d-914619642094">
The bad_mux's behavior is analyzed on GTKWave                            
<img width="1362" alt="Screenshot 2024-05-26 at 2 49 01 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/cb432d9f-023b-4b28-ac59-c25e9abff098">
                            </pre>
                            <p>Step 4</p>
                            <pre>
Invoke Yosys by using command yosys
<img width="815" alt="Screenshot 2024-05-26 at 2 49 12 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/3a80b296-11d9-4150-ae87-17d6d2c971d1">
                            </pre>
                            <p>Step 5</p>
                            <pre>
Read the library using read_liberty
<img width="731" alt="Screenshot 2024-05-26 at 2 49 25 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4e9fade6-5f81-41ac-a5d1-f3dbf9c35e98">
                            </pre>
                            <p>Step 6</p>
                            <pre>
Read the bad_mux.v using read_verilog
<img width="756" alt="Screenshot 2024-05-26 at 2 49 40 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/3d74ecc6-a028-4c2f-bf0b-9567d5270b26">
                            </pre>
                            <p>Step 7</p>
                            <pre>
Define the module that needs to be synthesized
<img width="268" alt="Screenshot 2024-05-26 at 2 50 33 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/ac8165b4-4a23-46ad-8196-242af9a8a9a8">
                            </pre>
                            <p>Step 8</p>
                            <pre>
Generate the netlist using abc command
<img width="612" alt="Screenshot 2024-05-26 at 2 50 51 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/ca4dc976-5645-4c1f-878c-650c93136276">
                            </pre>
                            <p>Step 9</p>
                            <pre>
Write the netlist to bad_mux_net.v
<img width="364" alt="Screenshot 2024-05-26 at 2 51 09 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/87c4ebdf-95ff-4a34-847e-d49d5fe62e2a">
                            </pre>
                            <p>Step 10</p>
                            <pre>
Execute show to view the design
<img width="603" alt="Screenshot 2024-05-26 at 2 51 21 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/287737a1-3d21-4fde-8578-7a65d568b560">
                            </pre>
                            <p>Step 11</p>
                            <pre>
Exit yosys and load the bad_mux_net.v to iverilog.
<img width="1368" alt="Screenshot 2024-05-26 at 2 52 52 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/f4bc8f2c-e08a-4675-be9a-e905b879c28a">
                            </pre>
                            <p>Step 12</p>
                            <pre>
Execute a.out file.
<img width="835" alt="Screenshot 2024-05-26 at 2 53 02 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/7813fe2a-bb20-43de-9cf7-ec53c1459f51">
                            </pre>
                            <p>Step 13</p>
                            <pre>
Load the generated .vcd file into GTKWave
<img width="979" alt="Screenshot 2024-05-26 at 2 53 25 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/87f26967-6d37-45e9-963e-15d4107e767f">
                            </pre>
                            <p>Step 14</p>
                            <pre>
Observe the behavior of GLS of ternary_operator_mux due to Simulation Mismatch
<img width="1365" alt="Screenshot 2024-05-26 at 2 53 59 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/16ececd7-a724-43be-91a3-b394763a3678">
                            </pre>          
                        </details>
                    </li>
                </ul>
            </details>
        </li>
        <li>
            <details>
                <summary>Synthesis Simulation Mismatch</summary>
                <p>Step 1</p>
                <pre>
Load blocking_caveat.v & its testbench to Iverilog.
<img width="1232" alt="Screenshot 2024-05-26 at 3 20 01 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/98e89aec-e1e0-4371-9865-7f4d46970466">
                </pre>
                <p>Step 2</p>
                <pre>
Execute a.out file.
<img width="831" alt="Screenshot 2024-05-26 at 3 20 12 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/cabfbd67-5caf-467e-a4fe-36b9dd3613ae">
                </pre>
                <p>Step 3</p>
                <pre>
Load the .vcd file genrated into GTKWave.
<img width="1056" alt="Screenshot 2024-05-26 at 3 20 37 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/cd3b4d01-c92d-40e8-842f-e41fcb512967">
The blocking_caveat's behavior is analyzed on GTKWave                            
<img width="1362" alt="Screenshot 2024-05-26 at 3 21 17 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/0313fa9c-df40-46d2-9e31-0e18b9eb5b84">
                </pre>
                <p>Step 4</p>
                <pre>
Invoke Yosys by using command yosys
<img width="809" alt="Screenshot 2024-05-26 at 3 21 29 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/45aa14d6-5cc0-4a4d-b629-74bc87fda4fa">
                </pre>
                <p>Step 5</p>
                <pre>
Read the library using read_liberty
<img width="727" alt="Screenshot 2024-05-26 at 3 21 43 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/e4aa45a5-c575-4ecc-9fe6-e10553ba31da">
                </pre>
                <p>Step 6</p>
                <pre>
Read the blocking_caveat.v using read_verilog
<img width="709" alt="Screenshot 2024-05-26 at 3 21 55 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/2c84de8b-63bc-4394-b833-79569ab86d0c">
                </pre>
                <p>Step 7</p>
                <pre>
Define the module that needs to be synthesized
<img width="596" alt="Screenshot 2024-05-26 at 3 23 34 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/886c9129-5cf7-4622-8857-326ffef0597f">
                </pre>
                <p>Step 8</p>
                <pre>
Generate the netlist using abc command
<img width="615" alt="Screenshot 2024-05-26 at 3 24 14 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/3035c361-2a06-41ca-8363-5a40d4827d87">
                </pre>
                <p>Step 9</p>
                <pre>
Write the netlist to blocking_caveat_net.v
<img width="523" alt="Screenshot 2024-05-26 at 3 25 02 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/cef23d28-8d32-4c0c-941f-a0420db792d6">
                </pre>
                <p>Step 10</p>
                <pre>
Execute show to view the design
<img width="603" alt="Screenshot 2024-05-26 at 3 25 14 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/f05de1e7-98ce-4ac3-bbaa-fbf14a31336c">
                </pre>
                <p>Step 11</p>
                <pre>
Exit yosys and load the blocking_caveat_net.v to iverilog.
<img width="1370" alt="Screenshot 2024-05-26 at 3 27 05 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/db39196e-3fba-424f-95ad-6b2f2e069d03">
                </pre>
                <p>Step 12</p>
                <pre>
Execute a.out file.
<img width="832" alt="Screenshot 2024-05-26 at 3 27 14 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/897f10b8-5491-46d7-a7ff-ca0eac28123a">
                </pre>
                <p>Step 13</p>
                <pre>
Load the generated .vcd file into GTKWave
<img width="1059" alt="Screenshot 2024-05-26 at 3 27 38 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/2f6188c0-d262-48f1-a756-24f1c709de6f">
                </pre>
                <p>Step 14</p>
                <pre>
Observe the behavior of GLS of blocking_caveat due to Simulation Mismatch
<img width="1357" alt="Screenshot 2024-05-26 at 3 28 12 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4f2dcd1c-5d73-474e-bf68-12c82ff30205">
                </pre>
            </details>
        </li>
    </ul>
</details>
<!--End of Day 4-->




<details>
  <summary>🛠️ Day 5 - Design for Testability (DFT)</summary>
 
# **Design for Testability (DFT)**

### **What is DFT?**

Design for Testability (DFT) in VLSI involves techniques to ensure ICs can be effectively tested post-manufacturing, improving yield, reliability, and quality.

### **Why DFT?**

DFT is essential for:
- Ensuring high-quality, reliable ICs
- Efficient manufacturing and debugging
- Meeting industry standards
- Supporting testing of complex designs

### **Testing Levels:**
1. **Wafer-Level Testing:** Tests individual dies on a wafer.
2. **Package-Level Testing (Die-Level):** Tests ICs post-packaging.
3. **System-Level Testing (Board-Level):** Tests ICs within complete systems.

### **Pros and Cons of DFT:**

**Pros:**
- Enhanced Fault Detection
- Improved Product Quality and Reliability
- Cost Reduction
- Increased Yield
- Simplified Debugging and Diagnosis

**Cons:**
- Increased Design Complexity
- Area and Power Overhead
- Performance Impact
- Test Development Time
- Cost of Test Equipment

### **Basic Terminologies in DFT:**

- **Controllability:** Ease of setting a specific internal node to a desired logic value.
- **Observability:** Ability to observe internal states and outputs.
- **Fault:** An unintended defect causing incorrect behavior.
- **Error:** Deviation from expected behavior or functionality.
- **Failure:** Undesirable outcome during testing indicating a defect.
- **Fault Coverage:** Percentage of detectable faults in a circuit.
- **Defect Level:** Severity of defects and corresponding measures during testing.

### **DFT Techniques:**

![DFT Techniques](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/a731deb7-aac1-4d9a-8397-90830b5f05ff)

### **Scan Flip-Flops:**

A scan flip-flop is used in digital circuits for testing and debugging, enabling access to its input and output in scan mode.

![Scan Flip-Flop](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c43e74d8-2587-400b-bbcf-57ddf0601da8)

### **Scan Chain Techniques:**

1. **Scan Chain Configuration:**
   - **Serial Connectivity:** Scan flip-flops connected in series.
   - **Scan-In and Scan-Out:** Test patterns loaded through SI and read through SO.

2. **Shift Operation:**
   - **Shift Mode:** Serially loads test patterns into the scan chain.
   - **Test Pattern Application:** ATPG tools generate and load test patterns for fault detection.

3. **Benefits:**
   - **Improved Testability:** Direct access to internal states.
   - **Observability and Controllability:** Better control and observation during testing.
   - **Efficient Debugging:** Allows observation and analysis of internal signals.

4. **Implementation Considerations:**
   - **Scan Chain Insertion:** Inserted during synthesis using DFT-aware tools.
   - **Test Pattern Generation:** ATPG tools generate test patterns for fault coverage.

### **ATE Functionality:**

Automatic Test Equipment (ATE) is used for testing and validating ICs and PCBs. It integrates ATPG for generating test patterns and applies them to the device under test.

![ATE Functionality](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d1922705-9d20-41b2-8f0a-33597c34d472)

### **DFT Compiler Overview:**

![DFT Compiler Overview](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/00dee519-6ff3-4ccb-b087-591b6b242121)

![DFT Compiler](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/177f6250-e888-4787-876a-f816713a3424)

### **Scan Chain Length:**

The length of scan chains varies based on design complexity and testing requirements. Optimal length balances test coverage, test time, data volume, and manufacturability.

### **Waveform Drawing:**

For drawing the waveform of a given circuit:

![Waveform](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/cf6875fb-772b-4ea3-93ea-edae230c3902)

</details>


<details>
  <summary>🛠️Day 6 - Introduction to Logic Synthesis</summary>

# Introduction to Logic Synthesis

## Logic Synthesis

### What is Logic Synthesis?
- Converts RTL description of a circuit into a netlist of logic gates and their connections.
- **HDL Compiler**: Converts RTL to Generic Boolean (without timing info).
- **Design Compiler**: Converts Generic Boolean into Target Technology (with timing info).

### What is Design Compiler?
- **Design Compiler (DC)**: EDA tool by Synopsys for Synthesis.
- **Interfaces**: `dc_shell` (text), Design Vision (graphical).
- **File Formats**:
  - `.db`: Library files.
  - `.ddc`: Design information, used across various Synopsys tools.
  - **SDC**: Design constraints (power, timing, area), uses TCL script.

### Design Compiler Flow

1. Set & link .db
2. Read .v file
3. Read SDC
4. Integrate the Design
5. Synthesize
6. Report
7. Check Quality of Results (qor) files
8. Write Netlist

### Netlist & Libraries

- Design is written using standard cells (gates, mux, flops, etc.) in `.db`.
- **Target Library**: Database with standard cell information (area, pin names, timing).
- Multiple libraries can be appended using link library.

### Getting Started

- **Specify Libraries**: Target & link libraries.
- **File Formats**:
  - **GUI**: `design_vision`
  - **Non-GUI**: `dc_shell`
  - **Switching to GUI**: `gui_start/start_gui`
- `.synopsys_dc.setup`: Used for integrating libraries at startup.

### TCL Tips

- Track bracket types.
- Use `$` to refer a variable.
- No `$` needed when assigning a variable.
- `"*"`: Matches string of characters.

## Design Compiler Introduction

### PART 1: Invoking DC Basic Setup

1. **Go to work directory**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/05fd724f-6b7a-4b91-a7c9-d08cf0f2826f)
2. **Enable C Shell**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/ed468576-859b-4aec-a9be-1ac7bb2b786a)
3. **Invoke DC using `dc_shell`**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/e0006db5-dce9-4972-af55-d3affea258f9)
4. **Read the Design**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/fd189762-574c-42d0-b626-39d98ba2c467)
   - Note: The register information indicates a 1-bit Flip Flop.
5. **Read library**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/7bf6ba5d-d903-4ffd-86f2-751fbee19ad3)
6. **Set target library**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/8d9456a7-e591-4f7a-808e-47ebfc862a6f)
7. **Set link library**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/3ffff2e4-e67e-45d9-82d3-6e82bdd998be)
   - Note: Important to specify the library for design. `*` represents already loaded library.
8. **Check library locations with echo**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/fb84601c-ea20-423d-a05e-538e3e0213ad)
9. **Compile the design**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/ecfa5ff2-fcab-486f-816d-54320cf55bf4)
   ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/1d976cf9-7421-4ea4-b89b-8a49d5ecef6f)
   ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/5a2c6fd2-03d3-46a8-82e8-4fcb27105b2e)
10. **Write verilog**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/cc2249e8-5f39-48e0-bb51-69916001ec0f)
    - Note: `-f` refers to the format (verilog) of the file to be written.
11. **View the written netlist**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/66c21a3e-bb64-4e3e-b346-95b9e22b4133)
    ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/2487f1dc-b3ff-4064-8040-456244a9beae)

## PART 2: Intro to Design Vision

1. **Write .ddc in dc_shell**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d999cd31-d708-478c-a30a-61cc6ac2dd9d)
2. **Open new tab and enable C Shell**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/ed468576-859b-4aec-a9be-1ac7bb2b786a)
3. **Launch Design Compiler in GUI mode**: `design_vision`
   ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c7194e75-d314-4626-8ed2-9a7a52bd6565)
4. **Read .ddc in Design Vision GUI**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/993077c6-e525-496e-ba38-5e0ce8e7f094)
5. **Open Schematic View and double-click to view standard cells**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/128e2c23-4b41-44b3-a92d-a3271e1b0d28)

### Design Compiler Synopsys Setup

1. **Open `.synopsys_dc.setup` using gvim**: Write commands to target and link the library.
   ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/32977e52-3883-43fa-b7bb-639019991e78)
2. **Verify by invoking the DC shell and echoing libraries**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/19d5fa37-213a-492f-8f6b-7bb22ea870a6)

</details>
 
<details>
  <summary>🔧 TCL Scripting Lab</summary>

# TCL Scripting Lab

Welcome to the TCL Scripting Lab! Follow these steps to get started with basic and advanced TCL commands.

## Step 1: Basic Commands

Initialize and view variables:

- **Set a Variable:**
  ```tcl
  set i 5
  ```
- **View Variable:**
  ```tcl
  echo $i
  # or
  puts $i
  ```
- **Increment Variable:**
  ```tcl
  incr i
  ```

![Basic Commands](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/752afc67-4e5f-441e-a52c-b8a11207a042)

## Step 2: Run 'For' Loop

Execute a 'for' loop to iterate over a range of values.

![For Loop](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/ce149b55-1f4c-4ca5-91d6-810e3a00ee0b)

## Step 3: Run 'While' Loop

Use a 'while' loop to execute commands as long as a condition is true.

![While Loop](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d6633c9a-cab2-4760-b461-70575cef3605)

## Step 4: Creating a List

Create a list using the `set` command.

![Creating a List](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/ba50215b-7ed6-4dce-891a-c3d547b462e7)

## Step 5: Looping through the List

Iterate over the elements of a list.

![Looping through List](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/1c00b3f9-e4f0-4699-8ffb-2f7baf2b740a)

## Step 6: Looping through a Collection

View all AND gates in a .db file and instantiate variables.

![Looping through Collection](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/b2c05b88-bbc1-4b95-8cfa-9baf5ab63ca6)

> Note: `get_lib_cells`, `get_object_name`, and `foreach_in_collection` are used in Synopsys.

## Step 7: Creating a TCL Script from DC

Launch `gvim` from within DC and edit documents with TCL commands.

![Creating TCL Script](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/a4ebd9cd-da52-4d12-aa12-d48658468cbb)

Press `i` to enter 'insert mode' and edit your document. Save the file as `testing.tcl`.

## Step 8: Executing a TCL Script from DC

Source the saved file to execute it.

![Executing TCL Script](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/5e9b63e8-307d-44a1-be45-cce2e055e30e)

Check the output to ensure all commands in `myscript.tcl` are executed correctly.

![Output](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d526ccc5-adbb-4eff-b524-9e4662e4c80b)

</details>



