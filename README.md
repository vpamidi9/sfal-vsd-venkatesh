
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
    <summary>🛠️ System Set Up and Tools Installation</summary>
    
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
    <summary>🛠️ Introduction to Verilog RTL Design and Synthesis</summary>

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
	<summary>🛠️ Timing libs, Hierarchical vs Flat Synthesis and Efficient Flop Coding Styles </summary>

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
    <summary>🛠️ Combinational and Sequential Optimization</summary>
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
    <summary>🛠️ Gate Level Simulation, Synthesis Simulation Mismatch, and Blocking & Non-Blocking Statements </summary>
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
  <summary>🛠️ Design for Testability (DFT)</summary>
 
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
  <summary>🛠️ Introduction to Logic Synthesis</summary>

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

<details>
	<summary>Introduction to SoC </summary>

 
# System on Chip (SoC)

## What is SoC?

A System on Chip (SoC) is an integrated circuit (IC) that consolidates all components of a computer or other electronic systems into a single chip. These components typically include a central processing unit (CPU), memory, input/output ports, and secondary storage – all on a single substrate. SoCs are used in mobile devices, embedded systems, and increasingly in PCs and servers due to their compactness and efficiency.

## Advantages of SoC

1. **Compact Size**: SoCs integrate multiple components into a single chip, significantly reducing the overall size of the system.
2. **Power Efficiency**: With all components on a single chip, power consumption is reduced due to shorter interconnections and optimized power management.
3. **Performance**: Integration leads to faster communication between components, enhancing overall system performance.
4. **Cost-Effective**: Reducing the number of separate components and interconnects can lower manufacturing and assembly costs.
5. **Reliability**: Fewer components and interconnections mean fewer potential points of failure, increasing system reliability.

## Disadvantages of SoC

1. **Design Complexity**: Integrating multiple components on a single chip increases design complexity, requiring sophisticated tools and methodologies.
2. **Development Cost**: The initial development and design of SoCs can be costly, involving advanced fabrication processes.
3. **Limited Flexibility**: Once designed and manufactured, modifying or upgrading an SoC can be challenging compared to discrete component systems.
4. **Thermal Management**: High integration density can lead to thermal issues, requiring advanced cooling solutions.

## Examples of SoC

1. **Apple A14 Bionic**: Used in iPhones and iPads.
2. **Qualcomm Snapdragon 888**: Commonly used in high-end Android smartphones.
3. **NVIDIA Tegra X1**: Used in devices like the Nintendo Switch and various automotive applications.
4. **Samsung Exynos 2100**: Used in Samsung's Galaxy S21 series.
5. **Broadcom BCM2837**: Found in the Raspberry Pi 3.

## Popular Applications of SoC

1. **Mobile Devices**: Smartphones, tablets, and smartwatches.
2. **Embedded Systems**: IoT devices, automotive control systems, and industrial automation.
3. **Consumer Electronics**: Smart TVs, set-top boxes, and gaming consoles.
4. **Computing Devices**: Laptops, desktops, and servers.
5. **Networking Equipment**: Routers, switches, and modems.

## Types of SoC

1. **Application-Specific SoC (ASIC)**: Customized for specific applications like graphics processing or AI acceleration.
2. **Programmable SoC (PSoC)**: Combines programmable logic with traditional SoC components.
3. **Standard SoC**: General-purpose SoCs used in a wide range of applications, such as those found in mobile devices.

## SoC Design Flow

1. **Specification and Planning**:
    - Define the system requirements, including performance, power, area, and functionality.
    - Choose the target applications and markets.

2. **Architecture Design**:
    - Develop the overall architecture, including CPU, GPU, memory, and peripherals.
    - Define the interconnect scheme and data flow.

3. **Component Selection**:
    - Choose standard IP cores (e.g., processors, memory controllers) or design custom components.
    - Ensure compatibility and integration capability of all components.

4. **Integration and Verification**:
    - Integrate the chosen components into a unified design.
    - Perform extensive verification using simulation, emulation, and formal methods to ensure functionality and performance.

5. **Physical Design**:
    - Perform synthesis to convert the high-level design into a gate-level netlist.
    - Conduct floorplanning, placement, and routing to create the physical layout of the chip.
    - Optimize for power, performance, and area (PPA).

6. **Fabrication and Testing**:
    - Send the final design to a semiconductor foundry for fabrication.
    - Perform post-fabrication testing to ensure the chip meets specifications.

7. **Software Development**:
    - Develop and optimize software to run on the SoC, including drivers, operating systems, and applications.
    - Ensure seamless integration between hardware and software components.
  
      ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/684603bf-947f-4242-ac51-3f752b4b735f)


## SoC Structure

1. **Processing Elements**: CPUs, GPUs, and DSPs that handle computation.
2. **Memory Components**: SRAM, DRAM, and cache memory for data storage.
3. **Interconnects**: Buses, crossbars, and networks-on-chip (NoCs) for communication between components.
4. **Peripherals**: Input/output controllers, timers, and communication interfaces (e.g., USB, Ethernet).
5. **Power Management**: Circuits to manage power distribution and consumption.
6. **Security Modules**: Hardware encryption and secure boot capabilities.
7. **Analog Components**: ADCs, DACs, and other mixed-signal elements.

## Conclusion

System on Chip (SoC) technology offers significant advantages in terms of size, power efficiency, performance, and cost. However, it also presents challenges in design complexity, development cost, flexibility, and thermal management. Effective SoC design requires careful planning, integration, verification, and optimization to meet the desired specifications and achieve market success.

</details>

<details>
	
 <summary>Introduction to VSDBabySoC</summary>
 
# VSDBabySoC

## Overview

VSDBabySoC is a compact and robust RISCV-based System on Chip. The primary goal of designing this small SoC is to integrate and test three open-source IP cores together for the first time and to calibrate its analog components. VSDBabySoC includes an RVMYTH microprocessor, an 8x Phase-Locked Loop (PLL) for generating a stable clock, and a 10-bit Digital-to-Analog Converter (DAC) for communication with other analog devices.

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/76536a0c-2f25-488f-b73d-30d1210b3476)

## BabySoC Components

### RVMYTH
- **RVMYTH**: The RVMYTH core is a simple RISC V-based CPU designed for educational purposes and small-scale applications. It provides a practical example of a RISC-V processor implementation.

### PLL
- **Phase-Locked Loop (PLL)**: A phase-locked loop or PLL is a control system that generates an output signal whose phase is related to the phase of an input signal. PLLs are widely used for synchronization purposes, including clock generation and distribution.

### DAC
- **Digital-to-Analog Converter (DAC)**: A DAC is a system that converts a digital signal into an analog signal. DACs are widely used in modern communication systems, enabling the generation of digitally-defined transmission signals.

## What is a PLL?

A phase-locked loop (also phase lock loop or PLL) is a system that generates an output signal whose phase is related to its input. The two signals will have the same frequency and either no phase difference or a constant phase difference between them.

A PLL typically consists of the following components:
- **Phase Detector**: Compares the reference signal with the oscillator frequency and outputs an error signal.
- **Loop Filter**: Usually a low-pass filter that generates an error voltage from the error signal.
- **Voltage-Controlled Oscillator (VCO)**: Adjusts the oscillator frequency to lock to the input frequency, producing an output frequency equal to the input frequency with a constant phase shift.

A PLL may also include a frequency divider in its feedback loop to create an output that is a multiple of the reference frequency instead of one that is exactly equal to it.



![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/78a02602-6eac-4448-941f-598b2c212274)



## Why Off-Chip Clocks Can't Be Used All the Time?

Using off-chip clocks for all on-chip operations in VLSI design is often impractical due to several key factors:

### Clock Distribution Delays
The clock signal needs to be distributed to a multitude of blocks on the chip. If a single off-chip clock source is used, the long wires required for distribution can introduce significant delays.

### Clock Jitter
Off-chip clocks are more susceptible to clock jitter, which can degrade the performance and reliability of the system.

### Different Frequency Requirements
Various blocks on the chip may require different clock frequencies. For example, one block might need 200 MHz while another needs 100 MHz. It is challenging to accommodate these varying frequency requirements with a single off-chip clock source.

### Clock Accuracy (ppm)
Quartz crystals used for clock generation have a specified accuracy in parts per million (ppm). A higher ppm error implies greater deviation from the desired frequency, impacting timing precision in electronic systems.

### Frequency Stability
Frequency stability denotes the maximum frequency variation over the operating temperature range. Crystals with higher ppm errors may exhibit significant frequency variations with temperature changes, affecting the reliability of timing references, especially in extreme temperature conditions.

### Total Frequency Error
The total frequency error of a crystal is the sum of errors from frequency tolerance, frequency stability, and aging. A higher ppm error in any of these components can lead to a larger total frequency error, affecting the crystal's overall accuracy in maintaining precise timing references.

## Digital-to-Analog Converter (DAC)

A Digital-to-Analog Converter (DAC) converts a digital input signal into an analog output signal. The digital signal is represented by a binary code, a combination of bits 0 and 1.

### Components of a DAC
- **Binary Inputs**: The number of binary inputs of a DAC is generally a power of two.
- **Analog Output**: A single output representing the analog signal.

### Types of DACs
- **Weighted Resistor DAC**: Uses resistors with weighted values to convert the binary input into an analog output.

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/7969a5fa-6bb4-49c7-ba92-7bd2dc074439)
  
  

- **R-2R Ladder DAC**: Utilizes a repetitive structure of resistors with values of R and 2R to achieve the conversion.

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/3b134b8b-dd13-4b1a-b30b-71335d60e010)


In our VSDBabySoC design, we are using a 10-bit DAC.

</details>

<details>
	<summary>VSDBabySoC Modelling</summary>

 # VSDBabySoC Modelling Overview

## What is Modelling?

Modelling and Simulation (M&S) involves the use of physical or logical representations of systems to generate data that informs decisions and predictions. This approach is particularly prevalent in the VLSI domain where it aids in design, analysis, verification, and validation of electronic systems.

## Purpose of Modelling

System models are crafted to:
- Support the analysis and specification of systems.
- Aid in system design and architectural planning.
- Facilitate system verification and validation.
- Enhance communication related to system details and functionality.

## Modelling the VSDBabySoC

### System Overview

The VSDBabySoC is modeled to integrate and simulate the interactions between various IP cores within a system on a chip (SoC), which includes:
- **Initial Inputs**: Entry signals fed into the VSDBabySoC module.
- **Phase-Locked Loop (PLL)**: Responsible for generating the appropriate clock signals for the system operations.
- **RVMYTH Processor**: Utilizes the clock signals to execute instructions and generate values.
- **Digital-to-Analog Converter (DAC)**: Converts the digital signals produced by the RVMYTH into analog output signals.

### Components
1. **RVMYTH**: A digital processing unit modeled using Hardware Description Language (HDL).
2. **PLL**: An analog component that generates stable clock signals.
3. **DAC**: An analog component that converts digital signals into analog outputs.

### Challenges in Modelling Mixed Signal Blocks

While digital blocks like RVMYTH can be easily modeled and simulated using HDLs such as Verilog, analog components like DACs and PLLs present a unique challenge as Verilog cannot directly synthesize analog designs. To overcome this, we use `real` data-types in Verilog to simulate analog functionalities and verify their logical correctness.

### Repositories Used as References
- [RVMYTH Repository](https://github.com/shivanishah269/risc-v-core)
- [PLL Repository](https://github.com/vsdip/rvmyth_avsdpll_interface)
- [DAC Repository](https://github.com/vsdip/rvmyth_avsddac_interface)

### Tools and Process for Modelling
RVMYTH is developed using TL-Verilog. To integrate it into our SoC model, we use tools like SandPiper SaaS to compile and transform TL-Verilog code into standard Verilog.

#### Step-by-Step Installation and Setup
1. **Install Required Packages**
   ```bash
   pip3 install pyyaml click sandpiper-saas
   ```
2. **Clone the VSDBabySoC Repository**
   ```bash
   git clone https://github.com/manili/VSDBabySoC.git
    cd VSDBabySoC
   ```

3. **Compile and Simulate the SoC Design**
   ```bash
   sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
   ```
   **sandpiper-saas** is used to to translate .tlv (transaction level verilog) files to .v (verilog) files.

   **Navigate to the VSDBabySoC directory and compile the design using Icarus Verilog**
   
   ```bash
   iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
   ```

5. **Run the Simulation**
   Change to the output directory and execute the simulation to generate the VCD file:
   ```bash
   cd output
   ./pre_synth_sim.out
   ```

6. **Viewing the Waveform**
   Open the simulation waveform in GTKWave for analysis:
   ```bash
   gtkwave pre_synth_sim.out
   ```
<img width="1500" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/8a18adc6-70b2-42fc-bc65-8e8f39cf8320">


<img width="1500" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/04905917-6988-463e-afe8-d5c55592f645">




## Understanding the Simulation Waveform

The simulation waveform provides insights into various signals within the VSDBabySoC module:

- **CLK**: The input clock signal to the RVMYTH core, generated by the PLL.
- **reset**: An input reset signal to the RVMYTH core, sourced externally.
- **OUT**: The output signal from the VSDBabySoC module, typically provided by the DAC. Due to simulation constraints, this appears as a digital signal but represents an analog value in a real application.
- **RV_TO_DAC[9:0]**: A 10-bit output from the RVMYTH core's register #17, representing data sent to the DAC.
- **OUT (Real Data Type)**: This represents an analog output signal from the DAC, simulated as a real data type in Verilog.

### Key Points

- **Signal Origin**: The CLK and reset signals are crucial for the operational timing and initialization of the RVMYTH core.
- **Output Analysis**: The OUT signal, as well as RV_TO_DAC, show how digital values are processed and converted into analog outputs within the system, reflecting the functionality of digital-to-analog conversion.
- **Real Data Type Usage**: This simulation uses Verilog's real data types to mimic the behavior of analog signals, providing a closer approximation to how the system would perform in a real-world scenario.


</details>

<details>
	<summary> BabySoC Post-Synthesis Simulation/Gate Level Simulation </summary>

 # VSDBabySoC Simulation Guide

## Importance of Pre-Synthesis and Post-Synthesis Simulation

### Why Pre-Synthesis Simulation?

Pre-synthesis simulation, often referred to as RTL simulation, is crucial for verifying the functional correctness of a design before the synthesis process. This type of simulation:

- **Focuses on Functionality**: It checks the logic functionality according to the design and code written, without the influence of physical constraints and delays.
- **Verifies Design Intent**: Ensures that the written code behaves as expected in a zero-delay environment, where events typically occur precisely at the active clock edge.
- **Detects Logical Errors**: Helps identify issues in the design logic before moving on to the more complex stage of synthesis, making debugging simpler and less costly.

### Why Post-Synthesis Simulation?

Post-synthesis simulation, also known as gate-level simulation (GLS), is performed after the synthesis process and includes delays and physical characteristics:

- **Includes Gate Delays**: This simulation incorporates the actual delays of gates and interconnects modeled in the synthesized netlist, providing a more accurate depiction of circuit behavior under real conditions.
- **Verifies Timing and Functionality**: It helps to detect both functional and timing errors introduced during the synthesis process, such as setup and hold violations.
- **Confirms Implementation Fidelity**: Ensures that the synthesis process has faithfully translated the RTL design into a gate-level netlist without altering its intended functionality.
- **Checks for Mismatches**: Identifies mismatches that might occur due to incorrect operator usage or unintended latch inference, which are not apparent in RTL simulation.
- **Dynamic Behavior Verification**: Gate-level simulation is critical for verifying dynamic behaviors of the circuit, such as clock gating and power-on reset, which cannot be accurately checked using static timing analysis alone.

### GLS: A Brief Introduction

The term "gate level" refers to the netlist view of a circuit, typically produced after logic synthesis. This view includes:

- **Netlist**: A complete connection list consisting of gates and IP models that exhibit both functional and timing behavior.
- **Simulation Modes**: While GLS can be performed with zero delay, it is commonly executed in unit delay mode or full timing mode to reflect real operational conditions.
- **Enhanced Confidence**: Performing GLS boosts confidence in the physical implementation of a design, ensuring that it meets all specified operational and performance criteria.

By conducting both pre-synthesis and post-synthesis simulations, designers can ensure that the VSDBabySoC operates correctly from both a functional and a timing perspective, thus significantly reducing the risk of costly revisions and silicon respins.

---

# Synthesis Process for VSDBabySoC

To synthesize the VSDBabySoC design, we need to convert the standard cell libraries into a `.db` format that can be used by synthesis tools such as Synopsys Design Compiler. Here's how you can accomplish this using the Synopsys Library Compiler (lc_shell):

## Step-by-Step Guide to Convert Libraries to `.db` Format

### Setting Up Your Environment
Ensure you are in the correct directory where the libraries are stored:
```bash
cd /home/venkatesh/VSDBabySoC/src/lib
```

### Reading the Library
Load the library file into the Library Compiler:
```bash
read_lib avsddac.lib
```

### Writing to `.db` Format
Convert the library to `.db` format using the `write_lib` command:
```bash
write_lib -format db avsddac -output avsddac.db
```
<img width="1324" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/22c23f84-11a2-4ff7-9fba-69566841003f">



### Reading the Library
Load the library file into the Library Compiler:
```bash
read_lib avsdpll.lib
```

### Writing to `.db` Format
Convert the library to `.db` format using the `write_lib` command:
```bash
write_lib avsdpll -format db -output avsdpll.db
```
<img width="1092" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/a60b45f9-e55b-4f87-9483-648ab6a06a91">


### Reading the Library
Load the library file into the Library Compiler:
```bash
read_lib sky130_fd_sc_hd__tt_025C_1v80.lib
```

### Writing to `.db` Format
Convert the library to `.db` format using the `write_lib` command:
```bash
write_lib sky130_fd_sc_hd__tt_025C_1v80 -format db  -output sky130_fd_sc_hd__tt_025C_1v80.db
```

<img width="1323" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/0fac3f1d-1d25-4249-aa09-26d71b3276a7">


This process will generate a `.db` file, which is a binary format that can be read by Synopsys tools for further synthesis and analysis steps. This file will be essential for the synthesis of the VSDBabySoC, as it contains all necessary information about the standard cells used in the design, including their timing, power, and area characteristics.

---

## Synthesis Commands

The following commands are used in the Synopsys Design Compiler environment to prepare and execute the synthesis of the VSDBabySoC design:

### Setting Libraries

- **Target Library**
  ```bash
  set target_library "/home/venkatesh/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.db"
  ```
- **Link Libraries**
  ```bash
  set link_library {* /home/venkatesh/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.db /home/venkatesh/VSDBabySoC/src/lib/avsdpll.db /home/venkatesh/VSDBabySoC/src/lib/avsddac.db}
  ```

### Setting Search Path

Specifies where the tool should look for design modules and files.

```bash
set search_path {/home/venkatesh/VSDBabySoC/src/include /home/venkatesh/VSDBabySoC/src/module}
```

### Breakdown of the Command

1. **set search_path**:
   - This command sets the search path for the tool to locate the design files. The `search_path` variable tells the tool where to look for the files it needs during processing.

2. **{...}**:
   - The curly braces `{}` enclose a list of directory paths. These paths are added to the search path for the tool.

3. **/home/venkatesh/VSDBabySoC/src/include**:
   - This is the first directory path specified in the search path. It typically contains include files or header files that are used in the design.

4. **/home/venkatesh/VSDBabySoC/src/module**:
   - This is the second directory path specified in the search path. It usually contains module source files or other Verilog files needed for the design.

### Purpose of the Command

The purpose of setting the search path is to inform the synthesis or simulation tool where to find all the necessary files required for the design. By specifying these directories, the tool can automatically locate and include the files during the design process.


### Reading Design Files

Loads all relevant design files and sets the top-level design.

```bash
read_file {sandpiper_gen.vh sandpiper.vh sp_default.vh sp_verilog.vh clk_gate.v rvmyth.v rvmyth_gen.v vsdbabysoc.v} -autoread -top vsdbabysoc
```


### Breakdown of the Command

1. **read_file**:
   - This is the primary command used to read or load files into the design tool. It tells the tool to read the specified files for further processing.

2. **{sandpiper_gen.vh sandpiper.vh sp_default.vh sp_verilog.vh clk_gate.v rvmyth.v rvmyth_gen.v vsdbabysoc.v}**:
   - This is a list of files to be read by the tool. The files are enclosed in curly braces `{}` and separated by spaces. They include:
     - `sandpiper_gen.vh`: Verilog header file likely containing parameter definitions or macro definitions.
     - `sandpiper.vh`: Another Verilog header file with potentially different or additional definitions.
     - `sp_default.vh`: A header file with default settings or configurations.
     - `sp_verilog.vh`: A Verilog header file, possibly containing utility macros or includes.
     - `clk_gate.v`: A Verilog source file, likely defining a clock gating module.
     - `rvmyth.v`: A Verilog source file, likely containing the definition of the RISC-V microcontroller core (RVMyth).
     - `rvmyth_gen.v`: A generated Verilog file, likely containing synthesized or pre-processed code for RVMyth.
     - `vsdbabysoc.v`: The top-level Verilog source file for the VSDBabySoC design.

3. **-autoread**:
   - This option tells the tool to automatically determine the order in which the files should be read. It helps the tool resolve dependencies between files without requiring manual ordering by the user.

4. **-top vsdbabysoc**:
   - This option specifies the top-level module of the design. The tool will treat `vsdbabysoc` as the root module for hierarchical processing and analysis. All other modules and files will be considered as part of the hierarchy under this top-level module.

### Purpose of the Command

The purpose of this command is to load all necessary Verilog source and header files into the design tool and to specify the top-level module of the design. This setup allows the tool to process the design hierarchy correctly and to prepare it for further steps such as synthesis, simulation, or implementation.

### Usage Context

This command is typically used in the setup phase of a design flow within EDA tools such as FPGA design tools (e.g., Xilinx Vivado, Intel Quartus), ASIC design tools (e.g., Synopsys Design Compiler, Cadence Genus), or simulation tools (e.g., ModelSim, QuestaSim).

- **Read and Parse Files**: The command ensures all necessary files are read and parsed by the tool.
- **Dependency Resolution**: The `-autoread` option helps the tool automatically resolve dependencies between the included files.
- **Top-Level Specification**: The `-top vsdbabysoc` option tells the tool which module to treat as the top-level module, guiding the tool in hierarchical design processing.

<img width="1324" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/f0f03d16-4837-476d-8af2-3c44c68ce701">



### Linking and Compilation

- **Linking**
  ```bash
  link
  ```

<img width="1324" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/9bdaa4c5-7b16-4c4f-91b9-3b63d473816b">


  This command links the design with the libraries and resolves all instantiations.

- **Ultra Compilation**
  ```bash
  compile_ultra
  ```

The purpose of the `compile_ultra` command is to perform a high-effort synthesis of a digital design. This process involves various optimization techniques and advanced algorithms to meet the design constraints more effectively than the standard `compile` command.

  
<img width="1324" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/3e5de126-f883-431b-8ccd-768c9b03d353">

---

### Writing Out the Netlist

Outputs the synthesized netlist in Verilog format to the specified location.

```bash
write_file -format verilog -hierarchy -output /home/venkatesh/VSDBabySoC/output/vsdbabysoc_net.v
```

### Breakdown of the Command

1. **write_file**:
   - This is the primary command used to write a file. It is typically used in synthesis or design tools to export a design into a file.

2. **-format verilog**:
   - This option specifies the format of the output file. In this case, `verilog` indicates that the file should be written in Verilog format.

3. **-hierarchy**:
   - This option indicates that the file should include the hierarchical structure of the design. It means the generated Verilog file will contain the full hierarchy, showing how modules are instantiated within other modules.

4. **-output /home/venkatesh/VSDBabySoC/output/vsdbabysoc_net.v**:
   - This specifies the path and filename for the output file. The `-output` option indicates that the next argument is the location where the file should be saved. In this case, the file will be saved as `vsdbabysoc_net.v` in the specified directory.

### Purpose of the Command

The purpose of this command is to export a design into a Verilog netlist file with hierarchical information included. This is useful for various purposes, such as:

- **Post-Synthesis Simulation**: The generated netlist can be used to simulate the design after synthesis to verify its behavior with gate-level accuracy.
- **Documentation**: The hierarchical netlist can serve as a detailed document of the design structure.
- **Verification**: The netlist can be used as an input to formal verification tools to ensure that the synthesized design matches the intended high-level design.

### Usage Context

This command is often used in electronic design automation (EDA) tools such as synthesis tools (e.g., Synopsys Design Compiler, Cadence Genus) or in scripts that automate parts of the design flow. It takes the synthesized design and writes it out as a Verilog netlist file, including all hierarchical information, which can then be used for further simulation, verification, or documentation purposes.

<img width="1324" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/1f2eb488-b28e-47d9-bbdc-798217f3217b">

---

## Post-Synthesis Simulation Commands

Execute the following commands in the `/VSDBabySoC` directory to perform the post-synthesis simulation:

### Compile the Design

Use `iverilog` to compile the design files, specifying functional and unit delay definitions. This command generates the simulation executable `post_synth_sim.out`.

```bash
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 -o ./output/post_synth_sim.out ./src/gls_model/primitives.v ./src/gls_model/sky130_fd_sc_hd.v ./output/vsdbabysoc_net.v ./src/module/avsdpll.v ./src/module/avsddac.v ./src/module/testbench.v
```

### Breakdown of the Command

1. **iverilog**:
   - This is the command to invoke Icarus Verilog, the tool used for compiling Verilog source files into a simulation executable.

2. **-DFUNCTIONAL**:
   - This defines a macro called `FUNCTIONAL` during compilation. It is often used to conditionally include or exclude parts of the code, allowing for different versions or modes of the design (e.g., functional simulation vs. synthesis).

3. **-DUNIT_DELAY=#1**:
   - This defines a macro `UNIT_DELAY` with a value of `#1`. This can be used to standardize delays across the simulation. The `#1` syntax indicates a time unit delay of 1 time unit, often used for simplifying timing analysis and simulations.

4. **-o ./output/post_synth_sim.out**:
   - This specifies the output file for the compiled simulation. The `-o` flag indicates that the next argument is the name of the output file. In this case, `./output/post_synth_sim.out` is the file where the compiled simulation will be written.

5. **./src/gls_model/primitives.v**:
   - This is one of the Verilog source files included in the compilation. It likely contains basic primitives used by the design.

6. **./src/gls_model/sky130_fd_sc_hd.v**:
   - This file probably contains the standard cell definitions from the SkyWater 130nm PDK (Process Design Kit), specifically for the high-density standard cell library.

7. **./output/vsdbabysoc_net.v**:
   - This file contains the netlist of the `vsdbabysoc` design after synthesis. The netlist is a detailed list of the components and connections in the design, generated from the high-level Verilog code.

8. **./src/module/avsdpll.v**:
   - This file contains the Verilog code for the PLL (Phase-Locked Loop) module used in the design.

9. **./src/module/avsddac.v**:
   - This file contains the Verilog code for the DAC (Digital-to-Analog Converter) module used in the design.

10. **./src/module/testbench.v**:
    - This is the testbench file, which contains the code to simulate and verify the functionality of the design. It typically includes stimulus and expected results for the modules under test.

### Purpose of the Command
The purpose of this command is to compile various Verilog source files, including primitives, standard cells, synthesized netlist, specific modules (PLL and DAC), and a testbench, into a single simulation executable. This executable (`post_synth_sim.out`) can then be run to simulate the post-synthesis behavior of the design, including the functionality of the individual modules and the overall system, as described by the testbench. The `-DFUNCTIONAL` and `-DUNIT_DELAY=#1` flags are used to control the simulation mode and timing characteristics.

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/0ef482dd-4bbf-439e-916a-009a70f9fe77)


From the snippet, it appears we encountered errors during the elaboration phase of our Verilog simulation with Icarus Verilog. The errors indicate that specific ports (`GND`, `VDD`, `VSSA`, and `VDDA`) are not found in the modules `pll` and `dac`:

```
./output/vsdbabysoc_net.v:5479: error: port `GND` is not a port of pll.
./output/vsdbabysoc_net.v:5479: error: port `VDD` is not a port of pll.
./output/vsdbabysoc_net.v:5481: error: port `VSSA` is not a port of dac.
./output/vsdbabysoc_net.v:5481: error: port `VDDA` is not a port of dac.
4 error(s) during elaboration.
```

### Resolution Steps

1. **Verify Module Declarations**:
   - Ensure that the `pll` and `dac` modules in their respective Verilog files (`avsdpll.v` and `avsddac.v`) have the correct port declarations, including `GND`, `VDD`, `VSSA`, and `VDDA`.

2. **Check Connectivity in Netlist**:
   - Verify that the synthesized netlist (`vsdbabysoc_net.v`) correctly connects these ports to the `pll` and `dac` modules.

3. **Cross-Check with Library Files**:
   - Ensure that the ports `GND`, `VDD`, `VSSA`, and `VDDA` are defined in the corresponding library files (`avsdpll.lib` and `avsddac.lib` files) and match those expected by the synthesized netlist.

4. **Modify Verilog Files**:
   - If the ports are indeed missing, we can add them to the module declarations.

After modifying the Verilog files, we can re-run your simulation command:

```bash
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 -o ./output/post_synth_sim.out ./src/gls_model/primitives.v ./src/gls_model/sky130_fd_sc_hd.v ./output/vsdbabysoc_net.v ./src/module/avsdpll.v ./src/module/avsddac.v ./src/module/testbench.v
```

By ensuring the correct port declarations are present in our Verilog source files, we should resolve the elaboration errors and successfully simulate your design. If the issues persist, double-check the module instantiations in the netlist file and ensure all connections are properly defined.


### Run the Simulation

Navigate to the output directory and run the simulation executable to generate the `dump.vcd` file.

```bash
cd output
./post_synth_sim.out
```

### Open the Simulation Waveform

Use GTKWave to visualize the simulation results stored in `dump.vcd`.

```bash
gtkwave dump.vcd
```


## Observing the Results

The images below show that our post-synthesis (top) and pre-synthesis (bottom) simulation results are identical, confirming the functional equivalence of the design before and after synthesis.

 **Post-Synthesis:**

![Post-Synthesis Image 1](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d977ee32-04c1-40af-b8c3-f7711d300a2f)

![Post-Synthesis Image 2](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/9b4efa1a-57c7-49a2-81a0-98c202d275f1)

 **Pre-Synthesis:**

![Pre-Synthesis Image 1](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/8a18adc6-70b2-42fc-bc65-8e8f39cf8320)

![Pre-Synthesis Image 2](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/04905917-6988-463e-afe8-d5c55592f645)

*Post-synthesis simulation waveform (top) and pre-synthesis simulation waveform (bottom).*

---


## Summary

These steps validate that the post-synthesis behavior of the VSDBabySoC design matches the pre-synthesis expectations, ensuring the correctness of the synthesis process.

</details>


<details>
	<summary>Synopsys DC and Timing Analysis</summary>

 # Understanding PVT Corners in Semiconductor Design


## Introduction
In semiconductor design, ensuring the reliability and performance of integrated circuits (ICs) across various conditions is crucial. This README provides an overview of PVT (Process, Voltage, Temperature) corners, explaining what they are, their importance, and why they are used in design and testing.

## What is PVT?
PVT stands for Process, Voltage, and Temperature, three critical parameters that influence the behavior and performance of semiconductor devices. 

- **Process**: Refers to variations in the manufacturing process. No two chips are exactly the same due to slight differences in the fabrication process.
- **Voltage**: Refers to the operating voltage supplied to the device. Variations in supply voltage can affect the performance and power consumption of the IC.
- **Temperature**: Refers to the operating temperature of the device. Temperature changes can impact the speed, leakage, and overall functionality of the semiconductor.


![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/91e8ccab-56a7-4cd4-9678-96107aada0b8)


## PVT Corners
PVT corners are specific combinations of the extreme values of Process, Voltage, and Temperature used to simulate and test the performance of semiconductor devices under worst-case scenarios. These corners help in predicting the behavior of ICs in real-world conditions.

### Common PVT Corners:
1. **SS (Slow-Slow)**: Both the NMOS and PMOS transistors are slow. Represents the worst-case scenario for speed.
2. **FF (Fast-Fast)**: Both the NMOS and PMOS transistors are fast. Represents the best-case scenario for speed.
3. **TT (Typical-Typical)**: Represents typical or nominal process conditions.
4. **SF (Slow-Fast)**: NMOS is slow, and PMOS is fast.
5. **FS (Fast-Slow)**: NMOS is fast, and PMOS is slow.

### Voltage and Temperature Extremes:
- **Voltage Corners**: Nominal voltage, maximum voltage (e.g., +10% of nominal), and minimum voltage (e.g., -10% of nominal).
- **Temperature Corners**: Typical operating temperature (e.g., 25°C), high temperature (e.g., 125°C), and low temperature (e.g., -40°C).

By combining these extremes, designers create various PVT corners, such as:
- **SS, Nominal Voltage, High Temperature**
- **FF, Max Voltage, Low Temperature**
- **TT, Min Voltage, Typical Temperature**

## Why Corners?
PVT corners are essential for several reasons:

1. **Reliability**: Ensures the IC operates correctly under different environmental and manufacturing conditions.
2. **Performance Verification**: Validates that the IC meets speed, power, and functionality requirements across all conditions.
3. **Yield Improvement**: Helps in identifying potential failure points, thereby improving the overall yield of the manufacturing process.
4. **Optimization**: Assists in optimizing the design for power, performance, and area by understanding the limits of the IC's operation.


Understanding PVT corners with real-world examples can provide clarity on their impact on semiconductor device performance. Let's consider a practical scenario involving a digital circuit, such as a simple CMOS inverter, and examine how PVT corners affect its operation.

## PVT Corners in a CMOS Inverter Example

A CMOS inverter consists of an NMOS and a PMOS transistor. The performance of this inverter can vary significantly based on process, voltage, and temperature variations.

### 1. Process Variation

Process variations arise due to manufacturing inconsistencies, affecting transistor dimensions and threshold voltages. Typical process corners are:
- **SS (Slow-Slow)**: Both NMOS and PMOS transistors are slow, resulting in slower switching.
- **FF (Fast-Fast)**: Both NMOS and PMOS transistors are fast, resulting in faster switching.
- **TT (Typical-Typical)**: Represents nominal process conditions.

### 2. Voltage Variation

Voltage variations refer to changes in the supply voltage (VDD). Typical voltage corners include:
- **Min Voltage (e.g., 0.9V)**: Lower supply voltage, reducing the drive strength of transistors.
- **Nominal Voltage (e.g., 1.0V)**: Standard operating voltage.
- **Max Voltage (e.g., 1.1V)**: Higher supply voltage, increasing the drive strength of transistors.

### 3. Temperature Variation

Temperature variations affect the carrier mobility and threshold voltage. Typical temperature corners include:
- **Low Temperature (e.g., -40°C)**: Increases carrier mobility but may affect threshold voltage.
- **Typical Temperature (e.g., 25°C)**: Standard room temperature.
- **High Temperature (e.g., 125°C)**: Decreases carrier mobility and increases leakage currents.

### Combining PVT Corners

Let's see how combining these variations into PVT corners can impact the inverter's performance:

### Example Scenario

Assume we have a CMOS inverter with the following nominal characteristics at TT, Nominal Voltage, and Typical Temperature:
- **Propagation Delay (Tpd)**: 100 ps
- **Power Consumption**: 10 µW

#### PVT Corners Analysis

1. **SS, Min Voltage, High Temperature (Worst-Case Delay)**
   - Process: Slow transistors
   - Voltage: 0.9V
   - Temperature: 125°C
   - **Impact**:
     - Increased propagation delay (e.g., 150 ps)
     - Reduced drive strength
     - Increased leakage power

2. **FF, Max Voltage, Low Temperature (Best-Case Delay)**
   - Process: Fast transistors
   - Voltage: 1.1V
   - Temperature: -40°C
   - **Impact**:
     - Reduced propagation delay (e.g., 70 ps)
     - Increased drive strength
     - Reduced leakage power

3. **TT, Nominal Voltage, Typical Temperature (Nominal Case)**
   - Process: Typical transistors
   - Voltage: 1.0V
   - Temperature: 25°C
   - **Impact**:
     - Nominal propagation delay (100 ps)
     - Nominal power consumption

4. **SF, Nominal Voltage, Typical Temperature (Mixed Process)**
   - Process: Slow NMOS, Fast PMOS
   - Voltage: 1.0V
   - Temperature: 25°C
   - **Impact**:
     - Slightly increased delay (e.g., 110 ps)
     - Imbalanced rise and fall times

5. **FS, Nominal Voltage, Typical Temperature (Mixed Process)**
   - Process: Fast NMOS, Slow PMOS
   - Voltage: 1.0V
   - Temperature: 25°C
   - **Impact**:
     - Slightly decreased delay (e.g., 90 ps)
     - Imbalanced rise and fall times

### Summary of Impact

- **Propagation Delay**: Varies significantly across different PVT corners. Designers must ensure that the circuit meets timing requirements in the worst-case scenario.
- **Power Consumption**: Leakage power increases at higher temperatures and lower supply voltages. Dynamic power depends on the switching activity and supply voltage.
- **Reliability**: Ensuring the circuit operates correctly across all PVT corners is crucial for robust design.

### Conclusion

By analyzing the performance of a CMOS inverter across various PVT corners, we can understand how process, voltage, and temperature variations affect its operation. This approach helps designers ensure that their circuits are reliable and meet performance specifications under all possible conditions.



---

# Converting Timing Libraries from .lib to .db


## Steps

### 1. Download Timing Libraries

Download the timing libraries from the GitHub repository to the specified path on your local machine.

#### Download Path

`/home/venkatesh/VSDBabySoC/src/lib_files`

#### GitHub Repository

[SkyWater PDK Libraries - SKY130 High Density Standard Cells](https://github.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/tree/master/timing)

You can download the files manually or use the following command:

```sh
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ff_100C_1v65.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ff_n40C_1v76.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_n40C_1v28.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_n40C_1v60.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ff_100C_1v95.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ff_n40C_1v95.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_n40C_1v35.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_n40C_1v76.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ff_n40C_1v56.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_100C_1v40.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_n40C_1v40.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__tt_025C_1v80.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ff_n40C_1v65.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_100C_1v60.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_n40C_1v44.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__tt_100C_1v80.lib
```

### 2. Convert .lib Files to .db Files

Use the following TCL script to automate the conversion of `.lib` files to `.db` files. The script reads each `.lib` file from the source directory and writes the corresponding `.db` file to the target directory.

**Syntax to convert .lib to .db for each**

```sh
cd /home/venkatesh/VSDBabySoC/src/lib_files
lc_shell
read_lib sky130_fd_sc_hd__ff_100C_1v65.lib
write_lib sky130_fd_sc_hd__ff_100C_1v65 -format db -output sky130_fd_sc_hd__ff_100C_1v65.db
```
To process multiple `.lib` files manually would be very time-consuming, so I automated the conversion using a TCL script.

#### TCL Script

```tcl
# Base directory where .lib files are located
set base_dir /home/venkatesh/VSDBabySoC/src/lib_files

# Directory to store the converted .db files
set db_dir /home/venkatesh/VSDBabySoC/src/db_files # ( Already created a db_files directory)



# List of .lib files to be converted
set lib_files {
    sky130_fd_sc_hd__ff_100C_1v65.lib
    sky130_fd_sc_hd__ff_n40C_1v76.lib
    sky130_fd_sc_hd__ss_n40C_1v28.lib
    sky130_fd_sc_hd__ss_n40C_1v60.lib
    sky130_fd_sc_hd__ff_100C_1v95.lib
    sky130_fd_sc_hd__ff_n40C_1v95.lib
    sky130_fd_sc_hd__ss_n40C_1v35.lib
    sky130_fd_sc_hd__ss_n40C_1v76.lib
    sky130_fd_sc_hd__ff_n40C_1v56.lib
    sky130_fd_sc_hd__ss_100C_1v40.lib
    sky130_fd_sc_hd__ss_n40C_1v40.lib
    sky130_fd_sc_hd__tt_025C_1v80.lib
    sky130_fd_sc_hd__ff_n40C_1v65.lib
    sky130_fd_sc_hd__ss_100C_1v60.lib
    sky130_fd_sc_hd__ss_n40C_1v44.lib
    sky130_fd_sc_hd__tt_100C_1v80.lib
}

# Loop through each .lib file and convert to .db format
foreach lib_file $lib_files {
    # Construct the full path to the .lib file
    set lib_path $base_dir/$lib_file
    
    # Extract the base name (without extension) for output file naming
    set base_name [file rootname $lib_file]
    
    # Read the .lib file
    read_lib $lib_path
    
    # Write the .db file to the db_files directory
    write_lib $base_name -format db -output "$db_dir/${base_name}.db"
}

# Print completion message
echo "All .lib files are converted to .db format and stored in $db_dir."
```

<img width="1023" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/9a4be7d6-fbef-42a4-801b-921eb69007b9">


### Instructions to Run the TCL Script

1. **Ensure Directories Exist**:
   - Verify that the `/home/venkatesh/VSDBabySoC/src/lib_files` directory contains the downloaded `.lib` files.
   - The script will create the `/home/venkatesh/VSDBabySoC/src/db_files` directory if it does not exist.

2. **Save the Script**:
   Save the script as `convert_libs_to_db.tcl`.

3. **Execute the Script in LC Shell**:
   ```sh
   lc_shell source convert_libs_to_db.tcl
   ```

<img width="930" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d42ed010-7250-49ef-81e7-176dc0d6cd42">


# VSDBabySoC Synthesis Process

## Synthesis Commands

### Step-by-Step Process for ff_100C_1v65

```bash
set target_library /home/venkatesh/VSDBabySoC/src/db_files/sky130_fd_sc_hd__ff_100C_1v65.db
set link_library {* /home/venkatesh/VSDBabySoC/src/db_files/sky130_fd_sc_hd__ff_100C_1v65.db  /home/venkatesh/VSDBabySoC/src/lib/avsdpll.db /home/venkatesh/VSDBabySoC/src/lib/avsddac.db}
set search_path {/home/venkatesh/VSDBabySoC/src/include /home/venkatesh/VSDBabySoC/src/module}

read_file {sandpiper_gen.vh sandpiper.vh sp_default.vh sp_verilog.vh clk_gate.v rvmyth.v rvmyth_gen.v vsdbabysoc.v} -autoread -top vsdbabysoc
link
read_sdc /home/venkatesh/VSDBabySoC/src/sdc/vsdbabysoc_synthesis.sdc
compile_ultra
write_file -format verilog -hierarchy -output /home/venkatesh/VSDBabySoC/output/vsdbabysoc_net_ff_100C_1v65.v
report_qor > ../report/timing_report_ff_100C_1v65.txt

get_attribute [get_timing_paths -delay_type max -max_paths 1] slack
get_attribute [get_timing_paths -delay_type min -max_paths 1] slack
```

Repeat the above commands for every `.db` file by adjusting the `target_library` and `output` paths accordingly.

## Graphical Representation of WNS and WHS with Respect to PVT Corners

The data is plotted to visualize the Worst Negative Slack (WNS) and Worst Hold Slack (WHS) across different PVT (Process, Voltage, Temperature) corners:

<img width="272" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/a4bf9b3d-a7a7-4138-9117-1c963e339e13">


### Visualization

The graph below provides a visual comparison of the Worst Negative Slack (WNS) and Worst Hold Slack (WHS) across different PVT conditions:


![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/45da0dc4-53c9-42ed-a679-96e20ba92850)





The plot provides a clear visual comparison of WNS and WHS across different PVT conditions, helping to identify the conditions where timing issues might occur.


## Conclusion

By following these steps, you will download the required timing libraries and convert them from `.lib` to `.db` format using the provided automated TCL script. This process simplifies the handling of timing libraries, ensuring they are ready for use in your design workflows. Additionally, performing synthesis and timing analysis using different PVT corners allows for a comprehensive understanding of the design's performance under various conditions. The graphical representation of WNS and WHS aids in identifying potential timing issues and optimizing the design accordingly.

</details>

<details>
	<summary>VSDBabySoC: Floorplanning and Power Planning</summary>
	
 ### Physical Design Flow 

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c0551561-d567-4423-a6e0-3a6f776198ec)



# Physical Design Flow for VSDBabySoC

Physical Design is the process of translating the gate-level netlist into a physical layout. This layout consists of various metal shapes and sizes that can be drawn onto masks and manufactured on the silicon wafer.

The Physical Design process can be broken down into multiple stages, as illustrated below. It is often an iterative process where a number of optimizations are performed at each step to meet the design performance, area, and power requirements.

## Floorplanning

**Floorplanning** is the first step of physical design. The design is partitioned into various smaller subsystems based on the system architecture and design requirements. Floorplanning determines the aspect ratio and area of the layout. Here we create the placement rows for standard cells and fix the placement of I/Os around the boundary. Any macros in the design are also placed during the floorplan stage.

### Key Aspects of Floorplanning
- **Inputs**: Gate level netlist (.v), Physical & Logical Libraries (.lefs & .libs), Synopsys design constraints (.sdc), RC Tech File (TLU+ file), Technology File (.tf), Physical Partitioning information, and Floorplanning Parameters.
- **Outputs**: Die/Core Area, IO Pad Information, Placed Macros Information, Standard Cell Placement Areas, Power Grid Design, and Blockages.

### Types of Floorplan Techniques
1. **Abutted Floorplan**: Channel-less placement of blocks with no gaps.
2. **Non-Abutted Floorplan**: Blocks are placed with gaps and connected through routing nets.
3. **Mix of Both**: Combines abutted and non-abutted techniques.

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/76ca6fa7-3a6b-48b1-906e-c6ee8671ab8a)


### Floorplan Steps
1. Define Width and Height
2. IO Pin Placement
3. Power Planning
4. Macro Placement
5. Standard Cell Row Creation
6. Blockages Definition


![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/176980af-1e0b-456a-9827-a689996591b3)


## Logic Placement

**Logic Placement** involves placing all the standard cells in the design at legal locations. After placement, EDA tools perform several optimizations to improve placement and reduce congestion.

### Example
- **Inputs**: Floorplan outputs, design constraints.
- **Outputs**: Placed standard cells, optimized layout.

## Clock Tree Synthesis (CTS)

During Floorplanning & placement, the clock is considered an ideal network. **CTS** creates a clock network to distribute the clock signal to all flops, achieving zero/minimum skew.

### Example
- **Inputs**: Placed standard cells.
- **Outputs**: Clock tree network, buffers, and inverters.

## Routing

Once standard cells are placed and the clock network is synthesized, all the connecting data nets are laid out on the metal layers. **Routing** involves connecting these nets, followed by optimizations based on design timing requirements.

### Example
- **Inputs**: Placed standard cells, clock tree network.
- **Outputs**: Routed nets, optimized design.

## Timing Analysis & Signoff

After routing, **Static Timing Analysis (STA)** is performed. This step breaks down the design into timing paths, calculates signal propagation delays, and checks for timing violations.

### Example
- **Inputs**: Routed design.
- **Outputs**: Timing reports, identified violations, optimized design for timing.

## Physical Verification & Signoff

After routing, the layout must be verified to ensure correct electrical and logical functionality. **Physical Verification** includes various checks like DRC, LVS, ERC, and more.

### Example
- **Inputs**: Final routed design.
- **Outputs**: Verified design, GDSII/OASIS file for fabrication.


## Power Planning

**Power planning** creates a power distribution network (PDN) to provide power to all chip components. It ensures stable voltage, avoids electromigration and self-heating, and meets IR drop targets.


### Power Distribution Hierarchy
1. **Power Pads**: Bring power from outside the chip.
2. **Power Rings**: Around the core area.
3. **Power Stripes**: Distribute power across the core.
4. **Power Rails**: Connect power stripes to standard cells.

### Key Steps in Power Planning
1. **Calculating power pins required**.
2. **Determining power rings, stripes, and rails**.
3. **Sizing power rings and stripes**.
4. **Analyzing and mitigating IR drop**.


![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/cd8656e1-97c4-44cb-81ed-375cf3c9a2fa)

### Lab - Floorplanning of VSDBabySoC

#### Downloading Physical Design Collaterals

Clone the repositories to get the necessary technology and setup files:

```bash
git clone https://github.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/tree/master
git clone https://github.com/bharath19-gs/synopsys_ICC2flow_130nm
git clone https://github.com/kunalg123/icc2_workshop_collaterals
```

The Interconnect Technology Format (ITF) file is crucial for physical design. It contains information about conductor and dielectric layers, including:
- Thickness, minimum width, and minimum spacing of each conductor layer.
- Sheet resistance (RPSQ) of each conductor layer.
- Thickness and dielectric constant (ER) of each dielectric layer.
- Resistivity (RHO) and area (AREA) of each via layer.

To convert the `.itf` file to `.tluplus` format, use the following commands:
```bash
cd /home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/itf_files
grdgenxo -itf2TLUPlus -i skywater130.nominal.itf -o skywater130.nominal.tluplus
```
The `grdgenxo` command is part of the Synopsys tool suite and is specifically used for generating grid data files, such as TLU+ (Table Lookup) files, from Interconnect Technology Format (ITF) files. The command's name likely stands for **Grid Generator Extended Output** or something similar, reflecting its function of generating extended output files that are essential for accurate physical design and analysis in electronic design automation (EDA).

Here’s a detailed explanation of its components and typical usage:

### Components of `grdgenxo`

1. **Grid Generation:**
   The primary function of `grdgenxo` is to generate grid data that describes the electrical characteristics of the interconnect layers (metal layers and dielectrics) in a semiconductor process.

2. **Input Format - ITF:**
   The input to `grdgenxo` is usually an ITF (Interconnect Technology Format) file. This file contains detailed descriptions of the metal layers, including thickness, width, spacing, resistance per square (RPSQ), and dielectric properties.

3. **Output Format - TLU+:**
   The output is typically a TLU+ file, which provides a detailed table lookup for resistance and capacitance values across different conditions. These files are used by place-and-route tools and other EDA software to model the interconnects accurately.

### Typical Usage

- **Conversion Command:**
  The command to convert an ITF file to a TLU+ file typically looks like this:

  ```bash
  grdgenxo -itf2TLUPlus -i <input.itf> -o <output.tluplus>
  ```

  Here, `-itf2TLUPlus` specifies the type of conversion, `-i` specifies the input ITF file, and `-o` specifies the output TLU+ file.

### Example Workflow

1. **Prepare ITF File:**
   Ensure that the ITF file, which contains the technology-specific parameters for the metal and dielectric layers, is ready. This file should include details such as thickness, minimum width, minimum spacing, resistance per square, and dielectric constants.

2. **Run the Command:**
   Execute the `grdgenxo` command to generate the TLU+ file.

   ```bash
   grdgenxo -itf2TLUPlus -i skywater130.nominal.itf -o skywater130.nominal.tluplus
   ```

3. **Verify Output:**
   Check the output directory for the generated TLU+ file and verify its contents to ensure it includes the expected resistance and capacitance data.

4. **Integration with EDA Tools:**
   Use the generated TLU+ file in your physical design flow. Tools like Synopsys ICC2, Cadence Innovus, or others will use this file for accurate modeling of interconnects during placement, routing, and timing analysis.

### Summary

- **grdgenxo**: A command-line utility used for generating grid data files, particularly TLU+ files, from ITF files.
- **ITF File**: Input file containing detailed descriptions of metal layers and dielectric properties.
- **TLU+ File**: Output file providing a table lookup for resistance and capacitance values, used by EDA tools for accurate interconnect modeling.

By using `grdgenxo`, designers can ensure that their physical design tools have precise interconnect models, leading to improved performance, reliability, and manufacturability of the integrated circuits.


### Synthesis Flow for VSDBabySoC

The synthesis flow converts the high-level RTL design into a gate-level netlist. Below are the steps and commands used for synthesizing the VSDBabySoC design.

1. **Set the Target Library**: Specify the target library that contains the standard cell definitions.
    ```tcl
    set target_library /home/venkatesh/VSDBabySoC/src/db_files/sky130_fd_sc_hd__tt_025C_1v80.db
    ```

2. **Set the Link Library**: Define the libraries needed for linking, including the target library and additional libraries.
    ```tcl
    set link_library {* /home/venkatesh/VSDBabySoC/src/db_files/sky130_fd_sc_hd__tt_025C_1v80.db  /home/venkatesh/VSDBabySoC/src/lib/avsdpll.db /home/venkatesh/VSDBabySoC/src/lib/avsddac.db}
    ```

3. **Set the Search Path**: Define the paths where the synthesis tool should search for include files and modules.
    ```tcl
    set search_path {/home/venkatesh/VSDBabySoC/src/include /home/venkatesh/VSDBabySoC/src/module}
    ```

4. **Read the Design Files**: Read the Verilog files that make up the design.
    ```tcl
    read_file {sandpiper_gen.vh sandpiper.vh sp_default.vh sp_verilog.vh clk_gate.v rvmyth.v rvmyth_gen.v vsdbabysoc.v} -autoread -top vsdbabysoc
    ```

5. **Link the Design**: Link the design to ensure all references are resolved.
    ```tcl
    link
    ```

6. **Read the SDC File**: Read the Synopsys Design Constraints (SDC) file to apply timing constraints.
    ```tcl
    read_sdc /home/venkatesh/VSDBabySoC/src/sdc/vsdbabysoc_synthesis.sdc
    ```

7. **Compile the Design**: Perform the synthesis to generate the gate-level netlist.
    ```tcl
    compile_ultra
    ```

8. **Write the Netlist**: Write the synthesized netlist to a Verilog file.
    ```tcl
    write_file -format verilog -hierarchy -output /home/venkatesh/VSDBabySoC/output/vsdbabysoc_net_tt_025C_1v80.v
    ```

9. **Generate Reports**: Create reports for Quality of Results (QoR), power, and area.
    ```tcl
    report_qor > ./report/timing_post_synth.txt
    report_power > ./report/power_post_synth.rpt
    report_area > ./report/area_post_synth.rpt
    ```

10. **Write the Design**: Save the design in DDC format.
    ```tcl
    write -f ddc -out /home/venkatesh/VSDBabySoC/output/vsdbabysoc.ddc
    ```

---

By following these steps, the VSDBabySoC design is synthesized, and various reports are generated to evaluate the quality, power, and area of the synthesized design. This synthesis flow ensures that the design is optimized and ready for further stages in the physical design process.


### Schematics VSDBabySoC


<img width="1512" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/104fba22-e5d2-462e-a609-88314ab362f5">

---

<img width="1512" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/637e8b9b-f3e7-4b79-836c-68ab8e4c83e7">

---

<img width="1512" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/7a39b122-b320-4292-b7db-506876cee75b">


---

<img width="1512" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/23cf7a01-34fc-4f6e-9a42-d36e6e07b9db">

---

<img width="1512" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/927bfa59-c45f-4c4c-b8d8-af2d4159882a">

---

<img width="1512" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/275be4c9-bedb-4ad9-96b7-d308a921e57b">

---

# VSDBabySoC: Physical Design Collaterals and Floorplan Options


## Collaterals Setup

The collaterals can be set up in the following files located at `/home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/`:

- `compile_pg_example.tcl`
- `init_design.mcmm_example.auto_expanded.tcl`
- `init_design.read_parasitic_tech_example.tcl`
- `init_design.tech_setup.tcl`
- `pns_example.tcl`
- `top.tcl`
- `write_block_data.tcl`





### Modifications in `top.tcl`


  ![top.tcl - Graphic 1](https://github.com/user-attachments/assets/85ed3e0d-20d7-446b-94f0-a72a5c8175e6)


  ![top.tcl - Graphic 2](https://github.com/user-attachments/assets/a3694b0f-1f33-453f-8c19-62125f4b8317)


  ![top.tcl - Graphic 3](https://github.com/user-attachments/assets/aebbf70f-3232-4d05-8610-85bd6491af85)


  ![top.tcl - Graphic 4](https://github.com/user-attachments/assets/3bc90fa4-63e5-4ca6-bc26-0f376d6022db)


  ![top.tcl - Graphic 5](https://github.com/user-attachments/assets/dbf57242-3175-4821-88a6-fe9eb8d23360)

### Modifications in `icc2_common_setup.tcl`


  ![icc2_common_setup.tcl - Graphic 1](https://github.com/user-attachments/assets/d3118ca6-3cae-4361-be4c-db841ff51da1)


  ![icc2_common_setup.tcl - Graphic 2](https://github.com/user-attachments/assets/6ca6ebb4-9d96-4b15-8cd5-ed734e9b7ec4)


  ![icc2_common_setup.tcl - Graphic 3](https://github.com/user-attachments/assets/cb2d6350-d751-4abc-a0e6-0017b5fd521a)
  ![icc2_common_setup.tcl - Graphic 4](https://github.com/user-attachments/assets/eda22bf1-a1fe-418f-96bc-5ae0e42481a2)
  ![icc2_common_setup.tcl - Graphic 5](https://github.com/user-attachments/assets/7bf1df9e-f4ce-4ffc-baf7-3624f2ac589a)
  ![icc2_common_setup.tcl - Graphic 6](https://github.com/user-attachments/assets/ff4fef1d-0c1b-4ad3-9591-ae09a6319cff)

### Changes in `init_design.read_parasitic_tech_example.tcl`


  ![init_design.read_parasitic_tech_example.tcl](https://github.com/user-attachments/assets/49f2d88f-29ce-4785-a8fe-d38640a73b16)

### Changes in `init_design.mcmm_example.auto_expanded.tcl`

  ![init_design.mcmm_example.auto_expanded.tcl](https://github.com/user-attachments/assets/e38879d8-5e5a-4e61-9b6b-819b347fc5df)

### Changes to `pns_example.tcl`

  ![pns_example.tcl](https://github.com/user-attachments/assets/73f518e4-a5cd-4d92-a7ac-6f766bdd126e)

### Modifications in `icc2_dp_setup.tcl`

  ![icc2_dp_setup.tcl - Graphic 1](https://github.com/user-attachments/assets/880c597b-1dba-4eea-b564-081c98380330)
  ![icc2_dp_setup.tcl - Graphic 2](https://github.com/user-attachments/assets/dc902ab1-b86e-4c18-8a47-eddb73c83f13)
  ![icc2_dp_setup.tcl - Graphic 3](https://github.com/user-attachments/assets/3556831d-add1-4101-be70-287f3a81b283)

---

# Executing every stage in top.tcl to undertand the flow

<img width="1508" alt="Pasted Graphic" src="https://github.com/user-attachments/assets/7f921565-d224-4f0c-af49-2973bd56682a">


<img width="1508" alt="image" src="https://github.com/user-attachments/assets/0d659339-b093-4583-931e-4f70393f18bd">

**Additoionally we can see changes in gui in every stage**

<img width="1508" alt="image" src="https://github.com/user-attachments/assets/7f2b942f-fc0c-4dcd-b847-7b07ab7b4d3d">


## Explanation of Commands

### Sourcing Setup Scripts

1. **Source `icc2_common_setup.tcl` with Echo**
   ```tcl
   source -echo /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/icc2_common_setup.tcl
   ```
   - This command sources the `icc2_common_setup.tcl` script with the `-echo` option, which prints each command executed by the script to the terminal. This is useful for debugging and verifying the commands being run.

2. **Source `icc2_dp_setup.tcl` with Echo**
   ```tcl
   source -echo /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/icc2_dp_setup.tcl
   ```
   - Similar to the previous command, this sources the `icc2_dp_setup.tcl` script with the `-echo` option for transparency and debugging purposes.

### Conditional Deletion of Design Library

3. **Check if the Design Library Exists and Delete It**
   ```tcl
   if {[file exists ${WORK_DIR}/$DESIGN_LIBRARY]} {
      file delete -force ${WORK_DIR}/${DESIGN_LIBRARY}
   }
   ```
   - This conditional block checks if the design library directory exists in the `WORK_DIR`.
   - If it exists, it forcefully deletes the directory using `file delete -force`. This ensures that any previous design library is removed before creating a new one.

### NDM Library Creation

4. **Create Library Command Setup**
   ```tcl
   set create_lib_cmd "create_lib ${WORK_DIR}/$DESIGN_LIBRARY"
   ```
   - This sets up the base command for creating the design library. The command `create_lib` is used to create a new library in the specified `WORK_DIR` with the name `DESIGN_LIBRARY`.

5. **Append Technology File or Technology Library**
   ```tcl
   if {[file exists [which $TECH_FILE]]} {
      lappend create_lib_cmd -tech $TECH_FILE ;# recommended
   } elseif {$TECH_LIB != ""} {
      lappend create_lib_cmd -use_technology_lib $TECH_LIB ;# optional
   }
   ```
   - This block conditionally appends options to the `create_lib_cmd` based on the existence of the technology file or technology library.
   - If the `TECH_FILE` exists, it appends `-tech $TECH_FILE` to use the technology file.
   - If the `TECH_LIB` variable is not empty, it appends `-use_technology_lib $TECH_LIB` to use the specified technology library.

6. **Append Reference Libraries**
   ```tcl
   lappend create_lib_cmd -ref_libs $REFERENCE_LIBRARY
   ```
   - This appends the `-ref_libs` option with the `REFERENCE_LIBRARY` to the `create_lib_cmd`. The reference library is used as a basis for creating the new library.

7. **Print the Create Library Command**
   ```tcl
   puts "RM-info : $create_lib_cmd"
   ```
   - This prints the complete `create_lib_cmd` to the terminal for informational and debugging purposes.

8. **Evaluate and Execute the Create Library Command**
   ```tcl
   eval ${create_lib_cmd}
   ```
   - This executes the `create_lib_cmd` using the `eval` command, which parses and runs the complete command string that has been constructed.

These commands collectively initialize and configure the design environment by sourcing necessary setup scripts, conditionally removing any existing design libraries, and creating a new library with specified technology and reference libraries.

---

<img width="1508" alt="image" src="https://github.com/user-attachments/assets/d215b260-6744-46a1-880c-47e799de4b23">


<img width="1508" alt="image" src="https://github.com/user-attachments/assets/6eb8cf0e-56f6-4e18-a931-12dfce6fda77">


<img width="1508" alt="image" src="https://github.com/user-attachments/assets/c02d5060-5884-40d8-a7fa-820b73afd0dd">


## Explanation of Commands

### Reading Synthesized Verilog

1. **Conditionally Read Verilog Outline or Full Chip Verilog**
   ```tcl
   if {$DP_FLOW == "hier" && $BOTTOM_BLOCK_VIEW == "abstract"} {
      # Read in the DESIGN_NAME outline.  This will create the outline
      puts "RM-info : Reading verilog outline (${VERILOG_NETLIST_FILES})"
      read_verilog_outline -design ${DESIGN_NAME}/${INIT_DP_LABEL_NAME} -top ${DESIGN_NAME} ${VERILOG_NETLIST_FILES}
   } else {
      # Read in the full DESIGN_NAME.  This will create the DESIGN_NAME view in the database
      puts "RM-info : Reading full chip verilog (${VERILOG_NETLIST_FILES})"
      read_verilog -design ${DESIGN_NAME}/${INIT_DP_LABEL_NAME} -top ${DESIGN_NAME} ${VERILOG_NETLIST_FILES}
   }
   ```
   - This block checks if the design planning flow (`DP_FLOW`) is hierarchical (`hier`) and if the bottom block view is abstract (`abstract`).
   - If true, it reads the Verilog outline using the `read_verilog_outline` command, creating the design outline.
   - Otherwise, it reads the full chip Verilog using the `read_verilog` command, creating the complete design view in the database.
   - The `${VERILOG_NETLIST_FILES}` contains the file paths to the synthesized Verilog files, and `${DESIGN_NAME}` and `${INIT_DP_LABEL_NAME}` specify the design and initialization labels.

### Technology Setup for Routing Layer Direction, Offset, Site Default, and Site Symmetry

2. **Set Up Technology Information**
   ```tcl
   if {$TECH_FILE != "" || ($TECH_LIB != "" && !$TECH_LIB_INCLUDES_TECH_SETUP_INFO)} {
      if {[file exists [which $TCL_TECH_SETUP_FILE]]} {
         puts "RM-info : Sourcing [which $TCL_TECH_SETUP_FILE]"
         source -echo $TCL_TECH_SETUP_FILE
      } elseif {$TCL_TECH_SETUP_FILE != ""} {
         puts "RM-error : TCL_TECH_SETUP_FILE($TCL_TECH_SETUP_FILE) is invalid. Please correct it."
      }
   }
   ```
   - This block sets up technology information if a technology file (`TECH_FILE`) is specified or if a technology library (`TECH_LIB`) is used but does not include the necessary setup information.
   - If the `TCL_TECH_SETUP_FILE` exists, it is sourced using the `source -echo` command, and the file's path is printed to the terminal.
   - If the `TCL_TECH_SETUP_FILE` does not exist or is invalid, an error message is printed.

### Specifying a Tcl Script to Read TLU+ Files

3. **Read Parasitic Technology Setup**
   ```tcl
   if {[file exists [which $TCL_PARASITIC_SETUP_FILE]]} {
      puts "RM-info : Sourcing [which $TCL_PARASITIC_SETUP_FILE]"
      source -echo $TCL_PARASITIC_SETUP_FILE
   } elseif ($TCL_PARASITIC_SETUP_FILE != "") {
      puts "RM-error : TCL_PARASITIC_SETUP_FILE($TCL_PARASITIC_SETUP_FILE) is invalid. Please correct it."
   } else {
      puts "RM-info : No TLU plus files sourced, Parasitic library containing TLU+ must be included in library reference list"
   }
   ```
   - This block specifies a Tcl script to read in TLU+ files, which are used for parasitic extraction.
   - If the `TCL_PARASITIC_SETUP_FILE` exists, it is sourced using the `source -echo` command, and the file's path is printed to the terminal.
   - If the `TCL_PARASITIC_SETUP_FILE` does not exist or is invalid, an error message is printed.
   - If no TLU+ files are sourced, a message is printed indicating that a parasitic library containing TLU+ must be included in the library reference list.

These commands collectively handle the reading of synthesized Verilog files, setting up technology information, and sourcing parasitic technology setup scripts, ensuring a complete and accurate design setup for further processing.

---

<img width="1508" alt="image" src="https://github.com/user-attachments/assets/15b0b9f3-1d0f-4241-9788-78fbbfb33f13">

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/9301922e-a9cb-4a43-8e64-d7b563a80247">


<img width="1512" alt="image" src="https://github.com/user-attachments/assets/6e7ae189-3472-4813-a4a5-5da4e28edbb4">


## Explanation of Commands

### Routing Settings

1. **Set Maximum Routing Layer**
   ```tcl
   if {$MAX_ROUTING_LAYER != ""} {
      set_ignored_layers -max_routing_layer $MAX_ROUTING_LAYER
   }
   ```
   - This command checks if the `MAX_ROUTING_LAYER` variable is set (not an empty string).
   - If it is set, it specifies the maximum routing layer to be used in the design by calling the `set_ignored_layers` command with the `-max_routing_layer` option.

2. **Set Minimum Routing Layer**
   ```tcl
   if {$MIN_ROUTING_LAYER != ""} {
      set_ignored_layers -min_routing_layer $MIN_ROUTING_LAYER
   }
   ```
   - This command checks if the `MIN_ROUTING_LAYER` variable is set (not an empty string).
   - If it is set, it specifies the minimum routing layer to be used in the design by calling the `set_ignored_layers` command with the `-min_routing_layer` option.

### Check Design: Pre-Floorplanning

3. **Check Design**
   ```tcl
   if {$CHECK_DESIGN} {
      redirect -file ${REPORTS_DIR_INIT_DP}/check_design.pre_floorplan {
         check_design -ems_database check_design.pre_floorplan.ems -checks dp_pre_floorplan
      }
   }
   ```
   - This command block checks if the `CHECK_DESIGN` variable is set to true.
   - If it is true, it runs the `check_design` command to perform a design check before floorplanning.
   - The `redirect` command is used to save the output of the `check_design` command to a file located at `${REPORTS_DIR_INIT_DP}/check_design.pre_floorplan`.
   - The `check_design` command is run with the `-ems_database` option to specify an EMS database file (`check_design.pre_floorplan.ems`) and the `-checks dp_pre_floorplan` option to specify the pre-floorplanning checks.

These commands ensure that the routing layer constraints are set and that the design is checked for issues before proceeding with the floorplanning stage. The checks are redirected to a specified file for review and documentation.

---

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/5b0d5333-dbd9-4f81-a79d-18dc4cde2a6c">


<img width="1512" alt="image" src="https://github.com/user-attachments/assets/8b8be95c-7ad8-48d0-aec3-dbc5c89def48">


<img width="1403" alt="Pasted Graphic" src="https://github.com/user-attachments/assets/7f0b0c14-e4f1-46ad-a9e3-ac231ff064c0">


## Explanation of Commands

### Floorplanning

1. **Initialize Floorplan with Core Utilization and Offset**
   ```tcl
   initialize_floorplan -core_utilization 0.3 -core_offset {5}
   ```
   - This command initializes the floorplan with a specified core utilization and offset.
   - `-core_utilization 0.3` sets the core area utilization to 30%.
   - `-core_offset {5}` sets the offset from the core boundary to 5 units on all sides.

2. **Create Keepout Margin**
   ```tcl
   create_keepout_margin -type hard -outer {2 2 2 2} [get_cells -physical_context -filter design_type==macro]
   ```
   - This command creates a hard keepout margin around specified cells.
   - `-type hard` specifies the keepout margin type as hard, meaning no cells can be placed within this margin.
   - `-outer {2 2 2 2}` sets the outer margins to 2 units on all sides.
   - `[get_cells -physical_context -filter design_type==macro]` selects cells that are macros to apply the keepout margin around them.

3. **Save Library**
   ```tcl
   save_lib -all
   ```
   - This command saves all the libraries currently in use.
   - `-all` specifies that all libraries should be saved.

### Summary

These commands are used to set up and initialize the floorplan for the design. The `initialize_floorplan` command sets the core utilization and offset, ensuring that the core area is properly defined. The `create_keepout_margin` command creates a hard keepout margin around macro cells to prevent other cells from being placed too close to them, ensuring proper spacing and avoiding placement issues. Finally, the `save_lib -all` command saves all libraries to preserve the current state of the design and its configurations.

----

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/0320325a-6580-4570-b0fa-70cc17ee7a6e">

<img width="1243" alt="image" src="https://github.com/user-attachments/assets/d08b6218-055f-4e99-805b-a9255af7d06c">

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/c939f257-a855-4f6e-bc86-8b15cd348b0d">



## Explanation of Commands

### Power and Ground (PG) Pin Connections

1. **Print Information Message**
   ```tcl
   puts "RM-info : Running connect_pg_net -automatic on all blocks"
   ```
   - This command prints an information message to the terminal, indicating that the `connect_pg_net -automatic` command is being run on all blocks.
   - `puts` is used to display messages in Tcl.

2. **Connect PG Nets Automatically**
   ```tcl
   connect_pg_net -automatic -all_blocks
   ```
   - This command connects power and ground (PG) nets automatically for all blocks in the design.
   - `-automatic` specifies that the connections should be made automatically based on predefined rules.
   - `-all_blocks` indicates that the command should apply to all blocks in the design.

3. **Save Block with Specific Label**
   ```tcl
   save_block -force -label ${PRE_SHAPING_LABEL_NAME}
   ```
   - This command saves the current state of the block with a specific label.
   - `-force` ensures that the block is saved even if there are warnings or if the block already exists.
   - `-label ${PRE_SHAPING_LABEL_NAME}` assigns the specified label to the saved block, allowing it to be easily identified later.

4. **Save All Libraries**
   ```tcl
   save_lib -all
   ```
   - This command saves all the libraries currently in use.
   - `-all` specifies that all libraries should be saved.

### Summary

These commands are used to connect the power and ground (PG) nets for all blocks in the design and to save the current state of the blocks and libraries. The `connect_pg_net -automatic -all_blocks` command ensures that all blocks have their PG nets connected automatically based on predefined rules. The `save_block -force -label ${PRE_SHAPING_LABEL_NAME}` command saves the current state of the block with a specific label, allowing it to be easily referenced later. Finally, the `save_lib -all` command saves all libraries to preserve the current state of the design and its configurations.

---

## Explanation of Verilog Code of SKY130_FD_SC_HD__CONB_1_FUNCTIONAL_V


<img width="1419" alt="image" src="https://github.com/user-attachments/assets/bb390e04-2771-4116-bf51-85be85b77e90">



<img width="861" alt="image" src="https://github.com/user-attachments/assets/df03ca09-2198-421b-9e8e-00b87a96b2b7">




### Header and Guard Clauses

1. **Header and Guard Clauses**
   ```verilog
   `ifndef SKY130_FD_SC_HD__CONB_1_FUNCTIONAL_V
   `define SKY130_FD_SC_HD__CONB_1_FUNCTIONAL_V
   ```
   - The `ifndef` directive checks if `SKY130_FD_SC_HD__CONB_1_FUNCTIONAL_V` is not defined.
   - If it is not defined, the `define` directive defines `SKY130_FD_SC_HD__CONB_1_FUNCTIONAL_V`.
   - This is used to prevent multiple inclusions of the same file, known as an include guard.

### Documentation and Timescale

2. **Documentation and Timescale**
   ```verilog
   /**
    * conb: Constant value, low, high outputs.
    *
    * Verilog simulation functional model.
    */

   `timescale 1ns / 1ps
   `default_nettype none
   ```
   - The comment block describes the module `conb` as having constant value low and high outputs.
   - `timescale 1ns / 1ps` specifies the time unit and precision for the simulation (1 nanosecond unit and 1 picosecond precision).
   - `default_nettype none` enforces that all nets must be explicitly declared, helping to avoid implicit net declarations which can lead to errors.

### Module Definition

3. **Module Definition**
   ```verilog
   `celldefine
   module sky130_fd_sc_hd__conb_1 (
       HI,
       LO
   );
   ```
   - `celldefine` marks the beginning of a cell definition, useful for CAD tools.
   - The module `sky130_fd_sc_hd__conb_1` is defined with two output ports: `HI` and `LO`.

### Module Ports and Internal Logic

4. **Module Ports and Internal Logic**
   ```verilog
       // Module ports
       output HI;
       output LO;

       //       Name       Output
       pullup   pullup0   (HI    );
       pulldown pulldown0 (LO    );
   ```
   - `output HI;` and `output LO;` declare `HI` and `LO` as output ports.
   - `pullup pullup0 (HI);` connects a pullup resistor to the `HI` output, ensuring it is constantly driven high.
   - `pulldown pulldown0 (LO);` connects a pulldown resistor to the `LO` output, ensuring it is constantly driven low.

### End of Module and Guard Clause

5. **End of Module and Guard Clause**
   ```verilog
   endmodule
   `endcelldefine

   `default_nettype wire
   ```
   - `endmodule` marks the end of the module definition.
   - `endcelldefine` marks the end of the cell definition.
   - `default_nettype wire` resets the default net type to `wire`.

### Include Guard End

6. **Include Guard End**
   ```verilog
   `endif // SKY130_FD_SC_HD__CONB_1_FUNCTIONAL_V
   ```
   - `endif` marks the end of the include guard, ensuring that the code between `ifndef` and `endif` is only included once.

### Summary

This Verilog code defines a module `sky130_fd_sc_hd__conb_1` with two outputs: `HI` and `LO`. The `HI` output is connected to a pullup resistor, making it constantly high, while the `LO` output is connected to a pulldown resistor, making it constantly low. The code includes guard clauses to prevent multiple inclusions and uses the `celldefine` directive for CAD tool compatibility. The timescale and default nettype are specified to ensure precise simulation behavior and explicit net declarations.


Making an output always low or high can be useful in various scenarios within digital design and circuit applications. Here are some reasons why you might want to have an output always low:

### Reasons for an Output Always Low

1. **Default State or Initialization:**
   - In some designs, certain signals need to be initialized to a known state. Setting an output to always low ensures that the connected components start in a known state, preventing unpredictable behavior at power-up or reset.

2. **Control Signals:**
   - Constant low signals can be used as control signals to disable or enable certain parts of a circuit. For example, a constant low might be used to keep a specific part of the circuit inactive until another condition is met.

3. **Logic Level Reference:**
   - Providing a constant low (or ground) signal can be useful as a reference for other components or circuits that need a steady low voltage reference.

4. **Testing and Debugging:**
   - During the testing phase, it might be necessary to force certain signals to a known state to isolate and debug parts of a circuit. A constant low output can be used to test how the circuit behaves under specific conditions.

5. **Unused Pins:**
   - Unused inputs on certain chips might need to be tied to a logic low to prevent them from floating, which can cause unpredictable behavior due to noise.

6. **Power Management:**
   - In power management scenarios, a constant low signal can be used to disable parts of a circuit, reducing power consumption when those parts are not needed.

### Example Use Case

Let's consider a scenario where you might need an output to always be low:

- **Reset Signal:** In many digital circuits, there is a reset signal that needs to be held low to reset the state of various flip-flops and other sequential elements. By providing a constant low signal, you can ensure that all parts of the circuit are properly reset before starting normal operation.


### Summary

Having an output always set to low is useful for initializing states, providing control signals, referencing logic levels, and various other scenarios in digital design. It ensures predictable and stable behavior in circuits where a known low state is required.

Choosing whether to keep an output always low or high depends on the specific requirements and context of your design. Both have their uses and advantages. Here's a comparison to help determine which might be best in different scenarios:

### Keeping Output Always Low

#### Advantages:
1. **Default State Initialization:**
   - Ensures certain components or signals start in a known low state, which might be required for proper initialization.

2. **Disabling Components:**
   - Can be used to disable certain parts of the circuit, ensuring they remain inactive until needed.

3. **Power Management:**
   - Often, keeping certain signals low can reduce power consumption, as some components consume less power in a low state.

4. **Ground Reference:**
   - Provides a stable ground reference for other parts of the circuit.

#### Use Cases:
- Reset signals that need to initialize the state of components.
- Control signals to keep parts of the circuit inactive.
- Unused inputs that need to be tied low to prevent floating.

### Keeping Output Always High

#### Advantages:
1. **Enabling Components:**
   - Can be used to enable certain parts of the circuit, ensuring they remain active or ready to operate.

2. **Default State Initialization:**
   - Ensures certain components or signals start in a known high state, which might be required for proper initialization.

3. **Voltage Reference:**
   - Provides a stable high voltage reference for other parts of the circuit.

#### Use Cases:
- Signals that need to be high to enable certain functionalities.
- Control signals to keep parts of the circuit active.
- Initialization signals that need to start in a high state.

### Comparison and Decision Criteria

1. **Component Requirements:**
   - Some components may require a high or low signal to be in a specific state (enabled, disabled, initialized).

2. **Power Consumption:**
   - Consider the power implications. Some designs might be more power-efficient with signals set to low, while others might not have a significant difference.

3. **Design Logic:**
   - Based on the logic design, determine whether the circuit operates correctly and efficiently with a constant high or low signal.

4. **Safety and Stability:**
   - Evaluate which state (high or low) ensures greater stability and safety for the overall circuit operation.

### Example Scenarios

1. **Reset Signal:**
   - Typically, reset signals are active low, meaning a low signal resets the circuit. In this case, having an output always low could be useful during initialization but would need to be controllable.

2. **Enable Signal:**
   - Many enable signals are active high, meaning a high signal enables the circuit. Here, having an output always high might be beneficial for default enable states.

### Conclusion

Neither always low nor always high is inherently better; the choice depends on the specific needs of your circuit and design. Here are some guiding points:
- **Use always low** if you need to initialize or disable components, provide a ground reference, or manage power efficiently.
- **Use always high** if you need to enable components, provide a voltage reference, or ensure certain signals start in an active state.

Ultimately, evaluate your specific design requirements and choose the state that best aligns with your circuit's functionality and performance goals.

---
<img width="1469" alt="image" src="https://github.com/user-attachments/assets/4df029a5-8724-4e32-b066-acd79e1a7c12">


<img width="1469" alt="image" src="https://github.com/user-attachments/assets/abf26f1e-8ddf-4063-b90c-e58cb6f694d3">



<img width="1469" alt="image" src="https://github.com/user-attachments/assets/9415a1dd-1fbf-4abe-94ea-6531e09d04e2">


## Explanation of Commands

### Memory Placement

1. **Check for Hard Macros and Report Constraints**
   ```tcl
   if [sizeof_collection [get_cells -hier -filter is_hard_macro==true -quiet]] {
      set all_macros [get_cells -hier -filter is_hard_macro==true]
      # Check top-level
      report_macro_constraints -allowed_orientations -preferred_location -alignment_grid -align_pins_to_tracks $all_macros > $REPORTS_DIR_PLACEMENT/report_macro_constraints.rpt
   }
   ```
   - This block checks if there are any hard macros in the design.
   - `get_cells -hier -filter is_hard_macro==true -quiet` retrieves all hard macros in the hierarchical design. The `sizeof_collection` function returns the number of hard macros found.
   - If any hard macros are found, the list of all hard macros is stored in the `all_macros` variable.
   - The `report_macro_constraints` command generates a report for macro constraints, including allowed orientations, preferred locations, alignment grid, and pin alignment to tracks. The report is saved to a file specified by `$REPORTS_DIR_PLACEMENT/report_macro_constraints.rpt`.

2. **Place Macros at User-Defined Locations**
   ```tcl
   if {$USE_INCREMENTAL_DATA && [file exists $OUTPUTS_DIR/preferred_macro_locations.tcl]} {
      source $OUTPUTS_DIR/preferred_macro_locations.tcl
   }
   ```
   - This block checks if the `USE_INCREMENTAL_DATA` variable is true and if the file `preferred_macro_locations.tcl` exists in the `$OUTPUTS_DIR` directory.
   - If both conditions are met, the `preferred_macro_locations.tcl` file is sourced, applying user-defined macro placement locations.

### Summary

These commands handle the placement of memory and hard macros in the design. The first block checks for the presence of hard macros and generates a report detailing their constraints and preferred locations. The second block applies user-defined macro placements if incremental data is being used and the corresponding Tcl file exists. This approach ensures that macros are placed according to design constraints and user preferences, facilitating efficient and optimal placement in the floorplan.

---


<img width="1473" alt="image" src="https://github.com/user-attachments/assets/29c508d7-fd69-4b7f-8a5d-cbc5e001023c">


## Explanation of Commands

### Configure Placement

1. **Set HOST_OPTIONS Based on DISTRIBUTED Flag**
   ```tcl
   if {$DISTRIBUTED} {
      set HOST_OPTIONS "-host_options block_script"
   } else {
      set HOST_OPTIONS ""
   }
   ```
   - This block checks if the `DISTRIBUTED` variable is set to true.
   - If `DISTRIBUTED` is true, it sets the `HOST_OPTIONS` variable to `-host_options block_script`.
   - If `DISTRIBUTED` is false, it sets `HOST_OPTIONS` to an empty string.

2. **Set CMD_OPTIONS with HOST_OPTIONS**
   ```tcl
   set CMD_OPTIONS "-floorplan $HOST_OPTIONS"
   ```
   - This command sets the `CMD_OPTIONS` variable.
   - It combines the `-floorplan` option with the value of `HOST_OPTIONS`.
   - If `HOST_OPTIONS` is set (e.g., `-host_options block_script`), `CMD_OPTIONS` will include this setting.
   - If `HOST_OPTIONS` is empty, `CMD_OPTIONS` will only include `-floorplan`.

### Summary

These commands configure the placement settings for the design based on whether the `DISTRIBUTED` flag is set. If the design is distributed, it adds specific host options to the command options. Otherwise, it proceeds with default settings. The `CMD_OPTIONS` variable is then constructed to include the necessary options for floorplanning, ensuring that the appropriate configurations are applied based on the `DISTRIBUTED` flag.

---

<img width="1118" alt="image" src="https://github.com/user-attachments/assets/0c1b3c58-19d2-40ae-bd50-dc8f59e0f1d4">


<img width="1493" alt="image" src="https://github.com/user-attachments/assets/3bd85bb1-3391-4f9d-abd3-d88ba8e3326f">


## Explanation of Commands

### Read Parasitic Files

1. **Check and Source Parasitic Setup File**
   ```tcl
   if {[file exists [which $TCL_PARASITIC_SETUP_FILE]]} {
      puts "RM-info : Sourcing [which $TCL_PARASITIC_SETUP_FILE]"
      source -echo $TCL_PARASITIC_SETUP_FILE
   } elseif {$TCL_PARASITIC_SETUP_FILE != ""} {
      puts "RM-error : TCL_PARASITIC_SETUP_FILE($TCL_PARASITIC_SETUP_FILE) is invalid. Please correct it."
   } else {
      puts "RM-info : No TLU plus files sourced, Parasitic library containing TLU+ must be included in library reference list"
   }
   ```
   - This block handles the sourcing of the parasitic setup file for the design.
   
2. **File Existence Check and Sourcing**
   ```tcl
   if {[file exists [which $TCL_PARASITIC_SETUP_FILE]]} {
      puts "RM-info : Sourcing [which $TCL_PARASITIC_SETUP_FILE]"
      source -echo $TCL_PARASITIC_SETUP_FILE
   }
   ```
   - This command checks if the `TCL_PARASITIC_SETUP_FILE` exists using the `file exists` and `which` commands.
   - If the file exists, it prints an informational message indicating that the file is being sourced.
   - The `source -echo` command sources the `TCL_PARASITIC_SETUP_FILE`, printing each command executed by the script for debugging purposes.

3. **Invalid File Check**
   ```tcl
   elseif {$TCL_PARASITIC_SETUP_FILE != ""} {
      puts "RM-error : TCL_PARASITIC_SETUP_FILE($TCL_PARASITIC_SETUP_FILE) is invalid. Please correct it."
   }
   ```
   - If the `TCL_PARASITIC_SETUP_FILE` variable is not empty but the file does not exist, it prints an error message indicating that the file is invalid and needs correction.

4. **No File Specified**
   ```tcl
   else {
      puts "RM-info : No TLU plus files sourced, Parasitic library containing TLU+ must be included in library reference list"
   }
   ```
   - If the `TCL_PARASITIC_SETUP_FILE` variable is empty, it prints an informational message indicating that no TLU+ files were sourced and that a parasitic library containing TLU+ must be included in the library reference list.

### Summary

These commands manage the process of reading parasitic setup files for the design. They check if the specified parasitic setup file exists and, if so, source it with detailed output for debugging. If the file is specified but does not exist, an error message is printed. If no file is specified, an informational message is printed, indicating the need for a parasitic library containing TLU+ in the library reference list. This ensures that the design has the necessary parasitic information for accurate timing and signal integrity analysis.

---

<img width="1302" alt="image" src="https://github.com/user-attachments/assets/02fe8b49-4c78-4943-ba3b-e3c3bdac538f">


<img width="949" alt="image" src="https://github.com/user-attachments/assets/61c5ca96-a885-4a16-bf3e-d70b36ed1bab">


<img width="796" alt="image" src="https://github.com/user-attachments/assets/8061a509-876b-4774-9011-4ae4f38b85df">


<img width="1129" alt="Pasted Graphic 1" src="https://github.com/user-attachments/assets/b784de9c-274e-427d-bccc-e57e56c019c3">


<img width="1439" alt="Pasted Graphic 2" src="https://github.com/user-attachments/assets/5e4d0805-255e-432d-917d-190dcc049c8f">



<img width="1439" alt="Pasted Graphic 3" src="https://github.com/user-attachments/assets/20522921-374c-4c5e-9c02-d37ab752402e">


<img width="1439" alt="Pasted Graphic 4" src="https://github.com/user-attachments/assets/5d306384-cca2-4b9d-adb5-8bb06e67c430">




## Explanation of Commands

### Read Constraints

1. **Check and Source MCMM Setup File**
   ```tcl
   if {[file exists $TCL_MCMM_SETUP_FILE]} {
      puts "RM-info : Loading TCL_MCMM_SETUP_FILE ($TCL_MCMM_SETUP_FILE)"
      source -echo $TCL_MCMM_SETUP_FILE
   } else {
      puts "RM-error : Cannot find TCL_MCMM_SETUP_FILE ($TCL_MCMM_SETUP_FILE)"
      error
   }
   ```
   - This block handles the loading of the Multi-Corner Multi-Mode (MCMM) setup file for the design.
   - It checks if the `TCL_MCMM_SETUP_FILE` exists using the `file exists` command.
   - If the file exists, it prints an informational message and sources the file with `source -echo`.
   - If the file does not exist, it prints an error message and raises an error to stop script execution.

### Placement Configuration

2. **Set Placement Options**
   ```tcl
   set plan.place.auto_generate_blockages true
   eval create_placement -effort high -timing_driven -congestion -floorplan
   legalize_placement
   ```
   - `set plan.place.auto_generate_blockages true`: Enables automatic generation of blockages during placement.
   - `eval create_placement -effort high -timing_driven -congestion -floorplan`: Creates the placement with high effort, considering timing, congestion, and the existing floorplan.
   - `legalize_placement`: Adjusts the placement to ensure legality, resolving any overlaps or other issues.

3. **Report Placement**
   ```tcl
   report_placement -physical_hierarchy_violations all -wirelength all -hard_macro_overlap -verbose high > $REPORTS_DIR_PLACEMENT/report_placement.rpt
   ```
   - Generates a detailed placement report, including physical hierarchy violations, wirelength, and hard macro overlap.
   - The report is saved to `$REPORTS_DIR_PLACEMENT/report_placement.rpt`.

### Macro Placement

4. **Write Macro Preferred Locations**
   ```tcl
   if [sizeof_collection [get_cells -hier -filter is_hard_macro==true -quiet]] {
      file delete -force $OUTPUTS_DIR/preferred_macro_locations.tcl
      set all_macros [get_cells -hier -filter is_hard_macro==true]
      derive_preferred_macro_locations $all_macros -file $OUTPUTS_DIR/preferred_macro_locations.tcl
   }
   ```
   - Checks if there are any hard macros in the design.
   - If hard macros are found, it deletes any existing `preferred_macro_locations.tcl` file in the `$OUTPUTS_DIR` directory.
   - The list of hard macros is stored in the `all_macros` variable.
   - The `derive_preferred_macro_locations` command generates preferred locations for the hard macros and saves them to `preferred_macro_locations.tcl`.

### Summary

These commands manage the process of reading MCMM constraints, configuring placement, and handling macro placement in the design. They ensure that the necessary constraints for MCMM analysis are loaded, create and legalize the placement with high effort considering timing and congestion, and generate a detailed placement report. Additionally, they manage the preferred locations for hard macros, ensuring optimal placement in subsequent runs. This comprehensive approach ensures accurate and efficient placement in the design flow.

---


<img width="776" alt="image" src="https://github.com/user-attachments/assets/6f2a65a7-359e-4abc-a4a6-8af1666bcaa4">


<img width="975" alt="image" src="https://github.com/user-attachments/assets/78b0da23-bd7e-4516-9368-46ddaf2d9da1">



## Explanation of Commands

### Fix All Shaped Blocks and Macros

1. **Check for Hard Macros**
   ```tcl
   if [sizeof_collection [get_cells -hier -filter is_hard_macro==true -quiet]] {
      set_attribute -quiet [get_cells -hier -filter is_hard_macro==true] status fixed
   }
   ```
   - This block checks if there are any hard macros in the design.
   - `get_cells -hier -filter is_hard_macro==true -quiet` retrieves all hard macros in the hierarchical design.
   - If hard macros are found, the `set_attribute -quiet [get_cells -hier -filter is_hard_macro==true] status fixed` command sets the `status` attribute of all hard macros to `fixed`.
   - Setting the `status` to `fixed` ensures that the placement of these hard macros is locked and they will not be moved in subsequent design stages.

2. **Save Block with Specific Label**
   ```tcl
   save_block -hier -force -label ${PLACEMENT_LABEL_NAME}
   ```
   - This command saves the current state of the hierarchical block with a specific label.
   - `-force` ensures that the block is saved even if there are warnings or if the block already exists.
   - `-label ${PLACEMENT_LABEL_NAME}` assigns the specified label to the saved block, allowing it to be easily identified later.

3. **Save All Libraries**
   ```tcl
   save_lib -all
   ```
   - This command saves all the libraries currently in use.
   - `-all` specifies that all libraries should be saved.

### Summary

These commands manage the process of fixing the placement of all hard macros and saving the current state of the design. The first block checks for the presence of hard macros and sets their status to `fixed` to lock their placement. The second command saves the hierarchical block with a specific label, ensuring that the current placement is preserved. Finally, the third command saves all libraries to maintain the current state of the design and its configurations. This approach ensures that the placement of critical components is maintained and can be reliably referenced in future design stages.


---

<img width="975" alt="image" src="https://github.com/user-attachments/assets/cc3188c2-1b63-45a0-8cc5-30f6f42502de">


<img width="975" alt="image" src="https://github.com/user-attachments/assets/67d7e752-7bcc-415f-835a-1a9f752f9fc6">


<img width="1438" alt="image" src="https://github.com/user-attachments/assets/d1578f53-1112-4b8e-ac60-c4b3bbb6200c">



<img width="616" alt="image" src="https://github.com/user-attachments/assets/18eb5d09-ae9f-414b-b99a-dc7ce8ef8efd">


## Explanation of Commands

### Create Power

1. **Source PNS File**
   ```tcl
   if {[file exists $TCL_PNS_FILE]} {
      puts "RM-info : Sourcing TCL_PNS_FILE ($TCL_PNS_FILE)"
      source -echo $TCL_PNS_FILE
   }
   ```
   - This block checks if the `TCL_PNS_FILE` exists.
   - If it exists, an informational message is printed, and the file is sourced using `source -echo`.

2. **PNS Characterization Flow**
   ```tcl
   if {$PNS_CHARACTERIZE_FLOW == "true" && $TCL_COMPILE_PG_FILE != ""} {
      puts "RM-info : RUNNING PNS CHARACTERIZATION FLOW because \$PNS_CHARACTERIZE_FLOW == true"
      characterize_block_pg -output block_pg -compile_pg_script $TCL_COMPILE_PG_FILE
      set_constraint_mapping_file ./block_pg/pg_mapfile
      if {$DISTRIBUTED} { 
         set HOST_OPTIONS "-host_options block_script"
      } else {
         set HOST_OPTIONS ""
      }
      puts "RM-info : Running run_block_compile_pg $HOST_OPTIONS"
      eval run_block_compile_pg ${HOST_OPTIONS}
   }
   ```
   - If the `PNS_CHARACTERIZE_FLOW` variable is set to "true" and `TCL_COMPILE_PG_FILE` is not empty, the PNS characterization flow is executed.
   - An informational message is printed, and `characterize_block_pg` is run to output to `block_pg` and compile the PG script specified by `TCL_COMPILE_PG_FILE`.
   - The constraint mapping file is set to `./block_pg/pg_mapfile`.
   - The `HOST_OPTIONS` variable is set based on whether the `DISTRIBUTED` variable is true or false.
   - An informational message is printed, and `run_block_compile_pg` is executed with the specified `HOST_OPTIONS`.

3. **Source Compile PG File**
   ```tcl
   } else {
      if {$TCL_COMPILE_PG_FILE != ""} {
         source $TCL_COMPILE_PG_FILE
      } else {
         puts "RM-info : No Power Networks Implemented as TCL_COMPILE_PG_FILE does not exist"
      }
   }
   ```
   - If the PNS characterization flow is not executed and `TCL_COMPILE_PG_FILE` is not empty, the `TCL_COMPILE_PG_FILE` is sourced.
   - If `TCL_COMPILE_PG_FILE` is empty, an informational message is printed, indicating that no power networks are implemented.

4. **Check PG Connectivity and DRC**
   ```tcl
   check_pg_connectivity -check_std_cell_pins none
   check_pg_drc -ignore_std_cells
   redirect -file $REPORTS_DIR_CREATE_POWER/check_mv_design.erc_mode {check_mv_design -erc_mode}
   redirect -file $REPORTS_DIR_CREATE_POWER/check_mv_design.power_connectivity {check_mv_design -power_connectivity}
   ```
   - `check_pg_connectivity -check_std_cell_pins none`: Checks PG connectivity, ignoring standard cell pins.
   - `check_pg_drc -ignore_std_cells`: Checks PG DRC, ignoring standard cells.
   - `redirect -file $REPORTS_DIR_CREATE_POWER/check_mv_design.erc_mode {check_mv_design -erc_mode}`: Redirects the output of `check_mv_design -erc_mode` to the specified report file.
   - `redirect -file $REPORTS_DIR_CREATE_POWER/check_mv_design.power_connectivity {check_mv_design -power_connectivity}`: Redirects the output of `check_mv_design -power_connectivity` to the specified report file.

5. **Save Block and Libraries**
   ```tcl
   save_block -hier -force -label ${CREATE_POWER_LABEL_NAME}
   save_lib -all
   ```
   - `save_block -hier -force -label ${CREATE_POWER_LABEL_NAME}`: Saves the current state of the hierarchical block with a specific label.
   - `save_lib -all`: Saves all the libraries currently in use.

### Summary

These commands handle the process of creating power networks in the design. They source the necessary PNS file, run the PNS characterization flow if required, and source the compile PG file if it exists. They also check PG connectivity and DRC, generating reports for these checks. Finally, the current state of the hierarchical block and all libraries are saved, ensuring that the power network setup is preserved and can be referenced in future design stages.

---



<img width="1478" alt="image" src="https://github.com/user-attachments/assets/8dda1222-9bf8-42fe-8f6f-1ba9440f1c54">



<img width="634" alt="image" src="https://github.com/user-attachments/assets/1aadb405-435b-41be-9122-fb395ce05be6">



<img width="1001" alt="image" src="https://github.com/user-attachments/assets/873890d0-8f54-41bb-834b-2aa6ca4465c7">




<img width="1216" alt="image" src="https://github.com/user-attachments/assets/8de26d4b-b689-474f-a7cb-f09202248031">



## Explanation of Commands

### Pin Placement

1. **Source Pin Constraint File**
   ```tcl
   if {[file exists [which $TCL_PIN_CONSTRAINT_FILE]] && !$PLACEMENT_PIN_CONSTRAINT_AWARE} {
      source -echo $TCL_PIN_CONSTRAINT_FILE
   }
   ```
   - This block checks if the `TCL_PIN_CONSTRAINT_FILE` exists and if the `PLACEMENT_PIN_CONSTRAINT_AWARE` variable is not set.
   - If both conditions are met, the `TCL_PIN_CONSTRAINT_FILE` is sourced using `source -echo`.

2. **Set Application Options**
   ```tcl
   set_app_options -as_user_default -list {route.global.timing_driven true}
   ```
   - This command sets application options to default to timing-driven global routing.

3. **Pre-Pin Placement Design Check**
   ```tcl
   if {$CHECK_DESIGN} {
      redirect -file ${REPORTS_DIR_PLACE_PINS}/check_design.pre_pin_placement {
         check_design -ems_database check_design.pre_pin_placement.ems -checks dp_pre_pin_placement
      }
   }
   ```
   - If `CHECK_DESIGN` is true, this block runs a design check before pin placement.
   - The results are redirected to `${REPORTS_DIR_PLACE_PINS}/check_design.pre_pin_placement`.

4. **Place Pins**
   ```tcl
   if {$PLACE_PINS_SELF} {
      place_pins -self
   }
   ```
   - If `PLACE_PINS_SELF` is true, this command places the pins using the `-self` option.

5. **Write Pin Constraints and Verify Placement**
   ```tcl
   if {$PLACE_PINS_SELF} {
      # Write top-level port constraint file based on actual port locations in the design for reuse during incremental run.
      write_pin_constraints -self -file_name $OUTPUTS_DIR/preferred_port_locations.tcl -physical_pin_constraint {side | offset | layer} -from_existing_pins

      # Verify Top-level Port Placement Results
      check_pin_placement -self -pre_route true -pin_spacing true -sides true -layers true -stacking true

      # Generate Top-level Port Placement Report
      report_pin_placement -self > $REPORTS_DIR_PLACE_PINS/report_port_placement.rpt
   }
   ```
   - If `PLACE_PINS_SELF` is true, this block writes the pin constraints based on the actual port locations to `preferred_port_locations.tcl`.
   - It verifies the top-level port placement results, checking pre-route pin spacing, sides, layers, and stacking.
   - It generates a report for the pin placement results, saved to `${REPORTS_DIR_PLACE_PINS}/report_port_placement.rpt`.

6. **Save Block and Libraries**
   ```tcl
   save_block -hier -force -label ${PLACE_PINS_LABEL_NAME}
   save_lib -all
   ```
   - `save_block -hier -force -label ${PLACE_PINS_LABEL_NAME}`: Saves the current state of the hierarchical block with a specific label.
   - `save_lib -all`: Saves all the libraries currently in use.

### Summary

These commands manage the pin placement process in the design. They source the pin constraint file if it exists, set application options, and run a pre-pin placement design check if required. They place the pins, write the pin constraints for reuse, verify the pin placement, and generate a placement report. Finally, they save the current state of the hierarchical block and all libraries, ensuring that the pin placement configuration is preserved for future design stages.


---


<img width="1219" alt="image" src="https://github.com/user-attachments/assets/2ba05818-a6a7-4d01-b076-ae29698ba4ab">


<img width="1051" alt="image" src="https://github.com/user-attachments/assets/a9d9cdd2-13ab-4de1-8272-d77ee830195d">



<img width="943" alt="image" src="https://github.com/user-attachments/assets/fa1cd299-9473-4d29-bd01-8bd89089762d">



<img width="943" alt="image" src="https://github.com/user-attachments/assets/aa227a78-e62f-4eac-9763-99a9933b77d0">


**Constraints used** 

<img width="943" alt="image" src="https://github.com/user-attachments/assets/3fa8d97e-76f2-4523-b003-b6794f2b6b7d">


**Report_timing**

<img width="943" alt="image" src="https://github.com/user-attachments/assets/c50617ef-75b5-45af-9766-127ed3a9ee84">

**report_constraints -all_violators**


<img width="984" alt="image" src="https://github.com/user-attachments/assets/107584d2-3f00-4577-bb60-93edddd73523">


<img width="928" alt="image" src="https://github.com/user-attachments/assets/646f2409-237a-4267-aca5-c2a231a9161f">


## Explanation of Commands

### Timing Estimation and Reporting

1. **Estimate Timing**
   ```tcl
   estimate_timing
   ```
   - This command performs a timing estimation for the design.

2. **Redirect Timing Report**
   ```tcl
   redirect -file $REPORTS_DIR_TIMING_ESTIMATION/${DESIGN_NAME}.post_estimated_timing.rpt {
      report_timing -corner estimated_corner -mode [all_modes]
   }
   ```
   - This block redirects the output of the `report_timing` command to a file.
   - `report_timing -corner estimated_corner -mode [all_modes]` generates a timing report for the estimated corner and all modes.
   - The report is saved to `$REPORTS_DIR_TIMING_ESTIMATION/${DESIGN_NAME}.post_estimated_timing.rpt`.

3. **Redirect Quality of Results (QoR) Report**
   ```tcl
   redirect -file $REPORTS_DIR_TIMING_ESTIMATION/${DESIGN_NAME}.post_estimated_timing.qor {
      report_qor -corner estimated_corner
   }
   ```
   - This block redirects the output of the `report_qor` command to a file.
   - `report_qor -corner estimated_corner` generates a QoR report for the estimated corner.
   - The report is saved to `$REPORTS_DIR_TIMING_ESTIMATION/${DESIGN_NAME}.post_estimated_timing.qor`.

4. **Redirect QoR Summary Report**
   ```tcl
   redirect -file $REPORTS_DIR_TIMING_ESTIMATION/${DESIGN_NAME}.post_estimated_timing.qor.sum {
      report_qor -summary
   }
   ```
   - This block redirects the output of the `report_qor -summary` command to a file.
   - `report_qor -summary` generates a summary QoR report.
   - The report is saved to `$REPORTS_DIR_TIMING_ESTIMATION/${DESIGN_NAME}.post_estimated_timing.qor.sum`.

### Save Block and Libraries

5. **Save Block with Specific Label**
   ```tcl
   save_block -hier -force -label ${TIMING_ESTIMATION_LABEL_NAME}
   ```
   - This command saves the current state of the hierarchical block with a specific label.
   - `-force` ensures that the block is saved even if there are warnings or if the block already exists.
   - `-label ${TIMING_ESTIMATION_LABEL_NAME}` assigns the specified label to the saved block, allowing it to be easily identified later.

6. **Save All Libraries**
   ```tcl
   save_lib -all
   ```
   - This command saves all the libraries currently in use.
   - `-all` specifies that all libraries should be saved.

### Summary

These commands manage the timing estimation process and generate detailed reports on the design's timing and quality of results (QoR). They estimate the timing, generate and redirect timing and QoR reports to specified files, and save the current state of the hierarchical block and all libraries. This ensures that the timing estimation results and the design configuration are preserved for future reference and analysis.

---

<img width="947" alt="image" src="https://github.com/user-attachments/assets/61e945dd-e7e0-4cbb-b7f7-b0bba0776457">

---


<img width="1512" alt="image" src="https://github.com/user-attachments/assets/6f351092-1d28-485d-8d45-88deea9ce26f">

---

<img width="649" alt="image" src="https://github.com/user-attachments/assets/66a56d28-a793-46b3-a045-efac14e604bc">

---

<img width="1433" alt="image" src="https://github.com/user-attachments/assets/cabfeccd-525c-43ff-a455-7251cea2fdbf">

---

<img width="1433" alt="Pasted Graphic 5" src="https://github.com/user-attachments/assets/c2012bc8-6395-45d6-af71-80fb194647a8">

---

<img width="1433" alt="image" src="https://github.com/user-attachments/assets/d26ebea7-3725-4e4e-92ec-864153fb0042">


## Explanation of Commands

### Place, Clock Tree Synthesis (CTS), and Route

1. **Set Host Options**
   ```tcl
   set_host_options -max_cores 8
   ```
   - This command sets the maximum number of cores to be used by the host to 8. This helps in parallelizing the process to utilize available CPU resources efficiently.

2. **Remove Estimated Corner**
   ```tcl
   remove_corners [get_corners estimated_corner]
   ```
   - This command removes the estimated corner from the design's corners. This is usually done to clean up any temporary or placeholder timing corners used during initial estimations.

3. **Set Application Options**
   ```tcl
   set_app_options -name place.coarse.continue_on_missing_scandef -value true
   ```
   - This command sets an application option to allow the placement process to continue even if a scan definition file is missing. This can be useful in scenarios where scan chains are not fully defined during initial placement.

4. **Perform Placement Optimization**
   ```tcl
   place_opt
   ```
   - This command performs placement optimization on the design. It adjusts the positions of standard cells to improve the design's performance and area utilization.

5. **Perform Clock Tree Synthesis (CTS) Optimization**
   ```tcl
   clock_opt
   ```
   - This command performs clock tree synthesis optimization. It ensures that the clock distribution network is optimized for minimal skew and optimal timing performance.

6. **Perform Routing**
   ```tcl
   route_auto -max_detail_route_iterations 5
   ```
   - This command performs automatic routing with a maximum of 5 iterations for detailed routing. Routing connects the physical wires between cells, adhering to design rules and optimizing for performance and manufacturability.

7. **Create Standard Cell Fillers**
   ```tcl
   set FILLER_CELLS [get_object_name [sort_collection -descending [get_lib_cells sky130_fd_sc_hd__fill*] area]]
   create_stdcell_fillers -lib_cells $FILLER_CELLS
   ```
   - `set FILLER_CELLS [get_object_name [sort_collection -descending [get_lib_cells sky130_fd_sc_hd__fill*] area]]`: This command identifies filler cells from the standard cell library, sorted in descending order by area.
   - `create_stdcell_fillers -lib_cells $FILLER_CELLS`: This command creates standard cell fillers using the identified filler cells. Filler cells are used to fill gaps between standard cells to ensure manufacturability and to maintain the integrity of the power and ground networks.

### Save Block and Libraries

8. **Save Block with Specific Label**
   ```tcl
   save_block -hier -force -label post_route
   ```
   - This command saves the current state of the hierarchical block with the label `post_route`.
   - `-force` ensures that the block is saved even if there are warnings or if the block already exists.
   - `-label post_route` assigns the label `post_route` to the saved block.

9. **Save All Libraries**
   ```tcl
   save_lib -all
   ```
   - This command saves all the libraries currently in use.
   - `-all` specifies that all libraries should be saved.

### Summary

These commands manage the process of placement, clock tree synthesis, and routing for the design. They set the maximum number of cores for parallel processing, remove temporary corners, set application options, and perform placement optimization. They also handle clock tree synthesis and automatic routing, create standard cell fillers, and save the current state of the hierarchical block and all libraries. This ensures that the design is optimized and all steps are properly documented and saved for future reference.

---

### report_timing

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/09153c82-112c-45ab-8e6d-f788d012844e">

---

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/ecfa9dc4-9fde-4a53-ab3f-0eb7544bf908">





</details>



