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
	<summary>Day 0 - System Set Up and Tools Installation </summary>

## Day 0: System Set Up and Tools Instllation

**Instructions on how to install tools:**

Download the Oracle Virtual Machine  - https://www.virtualbox.org/wiki/Downloads  

**System Configuration**    

    6GB RAM       
    50 GB HDD    
    Ubuntu 20.04+    
    4vCPU  
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/bde8d833-abdd-4a08-bbc3-fcc341b97227)

**Tool Installation**   

**1.Yosys**      

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

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/136cb330-2c9a-4d3b-93d0-8ec554c1f742)    

**2.iverilog**   
    
    $sudo apt-get update    
    $sudo apt-get install iverilog    

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/e9585bf3-4f92-4dbb-a18a-7d018e64467a)


**3.GTKwave**  
    
    $sudo apt-get update    
    $sudo apt install gtkwave    

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/5d789652-3560-4a5a-8c36-1c9556fea5be)

</details>

<details>
<summary>Day 1 - Introudction to Verilog RTL Design and Synthesis </summary>


## Day 1: Introduction to Verilog RTL Design and Synthesis

## Introduction to iVerilog OpenSource Simulator


**iverilog** : Icarus Verilog, commonly known as Iverilog, is an open-source tool used for the simulation and synthesis of digital circuits described in Verilog hardware description language (HDL).

Primarily, Iverilog is used to simulate Verilog designs, allowing designers to verify the functionality of their digital circuits before physical implementation.

Let's take good_mux.v design as an example

```
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

Let's see it's Testbench tb_good_mux.v

```
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
**Simulation of good_mux**
```
$ iverilog good_mux.v tb_good_mux.v
```
The output will be **a.out** which specifies the output file for the compiled simulation.
```
$ ./a.out
```
This command runs the simulation and generates a waveform dump file (in this case, tb_good_mux.vcd) as specified in the testbench.

To view the simulation results, use a waveform viewer like GTKWave. Open GTKWave and load the generated VCD file:
```
$ gtkwave tb_good_mux.vcd
```
GTKWave will open, allowing you to inspect the signals and verify the behavior of your design


![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d19c2602-6247-4ba2-8980-92dd27f52394)

## Introduction to Yosys

Yosys is an open-source framework for Verilog RTL synthesis. It is used primarily for converting high-level Verilog descriptions of digital circuits into gate-level netlists that can be implemented on FPGAs or used for ASIC design. Yosys is highly versatile and supports various front-end and back-end tools, making it a valuable tool for digital design and synthesis.

Inputs for Yosys tool: Design(.v), liberty(.lib)
Output: Netlist file (.net.v)

## Synthesis of a good_mux by opening Yosys:

**Read the Liberty source file**
```
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/615c393f-ca85-4d53-8ef2-0df77da89bed)

**Read the Verilog source file**
```
yosys> read_verilog good_mux.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/fd82b4ae-3614-407b-9ded-5969d212e413)


**Perform synthesis**
```
yosys> synth -top good_mux
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/49a325bf-c11c-4160-ba3c-540cc42b8066)

**Technology Mapping to the Design using *abc* tool which is integrated with Yosys**
```
yosys> abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/1d3345aa-668b-4753-bc33-f26b5c47cd89)


**Write the synthesized netlist to a Verilog file**
```
yosys> write_verilog -noattr good_mux.net.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d1506467-d9fe-4513-9bfc-108c4c73375b)

**View the generated gate level netlist**
```
yosys> show
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/8909cc50-ba0f-4ed7-b2b3-11782656e566)



</details>
<details>
	<summary>Day 2 - Timing libs, Hierarchical vs Flat Synthesis and Efficient Flop Coding Styles </summary>

## Day 2:Timing libs, Hierarchical vs Flat Synthesis and Efficient Flop Coding Styles

## Introduction to timing.lib

A .lib file, also known as a Liberty file, is a standard format used in the electronic design automation (EDA) industry to describe the timing, power, and area characteristics of the standard cells in a digital library.

**Key Contents of a .lib File:** Timing information, Power information, Area information, Opearating conditions, Pin descritions.

In this lab, we use **sky130_fd_sc_hd_tt_025C_1v80.lib**. Here's a breakdown of the filename and what each part signifies:

	sky130: Refers to the 130nm technology node provided by SkyWater Technology Foundry.

	fd: Stands for fully-depleted, indicating the type of process technology.

	sc: Stands for standard cell.

	hd: Stands for high-density standard cell library.

	tt: Typical process corner (typical-typical).

	025C: The temperature condition at which the library data is characterized (25°C).

	1v80: The operating voltage condition (1.8V).

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c8748f93-e472-492c-bdf3-a71b3a26eeb5)

## Hierarchical vs Flat Synthesis

**Hierarchial synthesis:** Hierarchical synthesis involves organizing the design into a hierarchy of modules or blocks, where each module represents a functional unit or a logical partition of the design.

**Read the Liberty source file**
```
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/615c393f-ca85-4d53-8ef2-0df77da89bed)

**Read the Verilog source file**
```
yosys> read_verilog multiple_modules.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/48d780ce-5c8b-4c4b-aa63-fc87c373b0d0)

**Perform synthesis**
```
yosys> synth -top multiple_modules
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/b5fdec51-ceaa-4de6-a425-5d5ca0c20d7a)

**Technology Mapping to the Design using *abc* tool which is integrated with Yosys**
```
yosys> abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/27ce5ad2-5528-4b44-b466-6a4f7bac390a)

**View the generated gate level netlist**
```
yosys> show multiple_modules
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/dc5ef610-0836-424d-93a4-9506f576a33e)



**Flat synthesis:** Flat synthesis involves synthesizing the entire design as a single, monolithic entity without hierarchical organization.

**To flatten the design**

```
yosys> flatten
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/41ea4f42-dfd8-4cfd-9b0a-215cec6f2a3c)

**To write a netlist to .v file**
```
yosys> write_verilog -noattr multiple_modules_flat.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/a33036ba-4305-48db-a714-d438269da691)

**To view the netlist(.v)**
```
yosys> !givim multiple_modules_flat.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/04d6e3f8-2307-452b-87f4-0ff5cbab5025)

**To view the flattened gate level netlist**
```
yosys> show
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/1d37fa5f-6bbf-402e-94d7-5232a2348038)


## Sub-Module level syntheis

**Read the Liberty source file**
```
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/615c393f-ca85-4d53-8ef2-0df77da89bed)

**Read the Verilog source file**
```
yosys> read_verilog multiple_modules.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/48d780ce-5c8b-4c4b-aa63-fc87c373b0d0)

**Perform synthesis**
```
yosys> synth -top sub_module1
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/10a7fcae-1eea-4453-88cf-12d8ff4fbe21)


**Technology Mapping to the Design using *abc* tool which is integrated with Yosys**
```
yosys> abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/2393c805-815c-45af-9046-187d3648a313)


**To view the generated gate level netlist**
```
yosys> show 
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/eee9dda1-5adb-46db-9f2b-f8a7c5203adc)


## Different Methods for Flip-Flop Coding and Performance Enhancement

**Why are flops necessary, and how do they mitigate glitches in the circuit?**

Glitches can manifest in digital circuits due to factors like signal propagation delays, noise interference, or timing discrepancies. Flops play a crucial role in preventing glitches during circuit operation through the following mechanisms:

**Synchronization:** Flops operate as edge-triggered components, responding exclusively to transitions in the input signal, such as rising or falling edges. This synchronization mechanism ensures that the output changes occur only at specific moments, minimizing the potential for glitches induced by transient signal fluctuations.

**Timing Control:** Flops are typically governed by a clock signal, ensuring that all circuit activities unfold synchronously. This synchronized operation eliminates timing discrepancies that could otherwise lead to glitches stemming from data arriving at disparate time

**Flop coding styles -**

	Simulation of Asynchronous Reset D-Flip Flop using iverilog followed by GTKWave
 
 ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/4398cdb4-a0ef-489b-a7cd-17b14378227a)


	Simulatyion of Asynchronous Set D-Flip Flop using iverilog followed by GTKWave

 ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/485cab5e-6c70-4176-9d5f-a4de986bf69d)

	Siumalation of Synchronous Reset D-Flip Flop using iverilog followed by GTKWave

 ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/7d1ced01-3428-4308-8d2f-ad07fe6d6afb)

	Synthesis of Asynchronous Reset D-Flip Flop using yosys, but here we have to map the Flip-FLops to the *dfflib* which is present in *sky130_fd_sc_hd_tt_025C_1v80.lib* 

 Here's the command for mapping the flipflops to the dfflib -  
 
```
 yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c784d898-40e0-4c8e-a2eb-ca1a7c55e61f)

	Synthesis of Asynchronous set D-Flip Flop using yosys
 
 ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/5ac9f787-30ca-4d22-860c-5a163fd10d40)

	Synthesis of Synchronous Reset D-Flip Flop using yosys
 
 ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c44d9314-3979-490e-9a33-131d8494b550)


 

</details>
<details>
<summary>Day 3 </summary>
 
</details>

<details>

<summary>Day 4 </summary>
 
</details>

<details>

<summary>Day 5 - Design for Testability (DFT) </summary>
 
**What is DFT?**

Design for Testability (DFT) in VLSI refers to a set of techniques and methodologies implemented in the design phase of ICs to ensure that they can be tested effectively after manufacturing. The goal of DFT is to enhance the testability of a design, allowing for the detection and diagnosis of faults, thereby improving yield, reliability, and quality of the ICs.

Why DFT?

DFT is essential for ensuring high-quality, reliable, and cost-effective ICs, enabling efficient manufacturing, debugging, and compliance with industry standards, and supporting the testing of increasingly complex designs.

We have 3 main levels of testing after a chip is being fabricated- 

Wafer-Level Testing: To test individual dies (chips) on a wafer before they are cut and packaged.

Package-Level Testing (Die-Level Testing): To test the ICs after they have been packaged but before they are integrated into the final product.

System-Level Testing (Board-Level Testing): To test the ICs as part of a complete system or printed circuit board (PCB) to ensure they work correctly in the final application.

We use DFT at the beginning of the design flow exactly during Synthesis.

Pros and Cons of DFT:

Pros - 
1. Enhanced Fault Detection
2. Improved Product Quality and Reliability
3. Cost Reduction
4. Increased Yield
5. Simplified Debugging and Diagnosis

Cons -
1. Increased Design Complexity
2. Area and Power Overhead
3. Performance Impact
4. Test Development Time
5. Cost of Test Equipment

Basic terminologies in DFT:

Controllability: It refers to the ease with which a specific internal node or signal within a digital circuit can be set to a desired logic value (0 or 1) through the primary inputs of the circuit. High controllability means that it is relatively easy to control the node's value, whereas low controllability indicates that it is difficult.

Observability: It refers to the ability to observe the internal states and outputs of a circuit to verify its correct operation and diagnose faults. It is a crucial concept in testing and debugging.

Fault: It refers to an unintended defect or anomaly in a digital circuit that causes it to behave incorrectly. Faults can arise from various sources such as manufacturing defects, design errors, or environmental stresses. Identifying and diagnosing these faults is a primary goal of DFT.

Error: It refers to any deviation or discrepancy from the expected behavior or functionality related to testing, test patterns, or test results.

Failure: It refers to an undesirable outcome or behavior during the testing process that indicates a defect or fault in the IC being tested.

Fault coverage: It measures the percentage of potential faults within a circuit that can be detected by the applied test patterns. Higher fault coverage indicates a more comprehensive and effective testing strategy.

Defect Level: It refers to the severity or criticality of defects that can occur in an IC and the corresponding measures taken to detect and address these defects during testing. 

DFT techniques:

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/a731deb7-aac1-4d9a-8397-90830b5f05ff)

Scan flip-flops:

A scan flip-flop, also known as a scan element or scan cell, is a specific type of flip-flop used in digital circuits for testing and debugging purposes, particularly in Design for Testability (DFT) methodologies. 

Functionality - 

Normal Operation:

In normal operation, a scan flip-flop behaves like a standard D flip-flop or T flip-flop, depending on its design.
It stores binary data (0 or 1) based on the clock signal and input data, similar to regular flip-flops used in sequential logic.

Scan Mode:

When a circuit enters scan mode (also called test mode or scan-in/scan-out mode), the behavior of scan flip-flops changes.
In scan mode, the scan flip-flop allows direct access to its input and output, bypassing the normal data path.

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c43e74d8-2587-400b-bbcf-57ddf0601da8)

Scan chain techniques:

Scan chain techniques are a key aspect of Design for Testability (DFT) in digital circuits, especially for sequential logic elements like flip-flops. They involve connecting multiple scan flip-flops in a chain to facilitate efficient testing and fault diagnosis. Here's an overview of scan chain techniques:

1. Scan Chain Configuration

Serial Connectivity:

Scan flip-flops are connected in series, forming a scan chain or shift register configuration.
Each scan flip-flop has a dedicated scan input (S) and scan output (Q') for serial data shifting.
Scan-In and Scan-Out:

The first scan flip-flop in the chain receives test patterns through its scan input (S), referred to as the scan-in (SI) pin.
The last scan flip-flop in the chain provides the shifted output through its scan output (Q') pin, known as the scan-out (SO) pin.

2. Shift Operation

Shift Mode:

During testing, the circuit enters scan mode, where test patterns are serially loaded into the scan chain.
The shift operation involves clocking the scan chain to shift test patterns or data through each scan flip-flop in the chain.

Test Pattern Application:

Test patterns generated by Automatic Test Pattern Generation (ATPG) tools or other test sources are loaded into the scan chain serially.
This allows efficient application of test patterns to the circuit for fault detection and diagnosis.

3. Benefits of Scan Chain Techniques

Improved Testability:

Scan chain techniques enhance the testability of sequential logic elements by providing direct access to internal states.
They enable thorough testing for manufacturing defects, functional verification, and fault coverage analysis.

Observability and Controllability:

Scan chains improve observability (ability to observe internal signals) and controllability (ability to control internal states) during testing.
They facilitate the testing of sequential paths, feedback loops, and hard-to-reach nodes that are challenging to test using traditional methods.

Efficient Debugging:

During debugging and fault diagnosis, scan chains allow designers to observe and analyze internal signals at different stages of the chain.
This enables efficient identification and localization of faults, aiding in debugging efforts and reducing time-to-market.

Support for DFT Techniques:

Scan chains are used in conjunction with other DFT techniques such as Built-In Self-Test (BIST), boundary scan (JTAG), and test pattern generation (ATPG) to achieve comprehensive test coverage.
They form the backbone of DFT strategies for digital circuits, especially in complex designs and integrated systems.


Implementation Considerations

Scan Chain Insertion:

Scan flip-flops and scan chains are inserted into the design during the synthesis phase using DFT-aware synthesis tools.
Proper placement and routing of scan chains are crucial to minimize area overhead and ensure signal integrity.

Test Pattern Generation:

ATPG tools generate test patterns targeting the scan chains to achieve desired fault coverage and test quality.
Scan chain configuration and test pattern application are verified through simulation and validation processes.

ATE functionality - 

Automatic Test Equipment (ATE) is a specialized electronic device used in the testing and validation of integrated circuits (ICs), printed circuit boards (PCBs), and electronic systems.

Test Pattern Generation

ATPG Integration:

ATE systems integrate Automatic Test Pattern Generation (ATPG) software or algorithms to generate test patterns for various types of faults.
ATPG tools create patterns targeting specific fault models (e.g., stuck-at faults, transition faults) to achieve high fault coverage.

Pattern Application:

ATE systems apply generated test patterns to the device under test (DUT), such as ICs or PCBs, using specialized test probes or interfaces.
Test patterns are applied serially or in parallel, depending on the test requirements and capabilities of the ATE system.

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d1922705-9d20-41b2-8f0a-33597c34d472)

DFT compiler overview - 

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/00dee519-6ff3-4ccb-b087-591b6b242121)



![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/177f6250-e888-4787-876a-f816713a3424)


How long can a Scan chain can be?

 Scan chains in modern designs can range from a few scan elements to several hundred or even thousands, depending on the complexity of the design and the specific testing requirements. Designers often perform simulations and analysis to determine the optimal scan chain length that balances test coverage, test time, data volume, and manufacturability considerations. Working closely with ATE vendors and considering industry best practices can also help in determining the appropriate scan chain length for a given design.

How to draw the waveform of the following circuit?

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/cf6875fb-772b-4ea3-93ea-edae230c3902)





</details>

