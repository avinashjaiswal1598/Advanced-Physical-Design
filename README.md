## Advanced Physical Design using OpenLane/sky130

![Screenshot from 2024-03-30 16-38-11](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/df19a038-657d-432c-857c-ff30be102c70)


## Introduction

### About OpenLane

[OpenLane](https://github.com/efabless/openlane2.git), an open-source digital RTL-to-GDSII flow, is frequently utilized in conjunction with [skywater130nm](https://github.com/google/skywater-pdk.git) process technology for integrated circuit (IC) design and implementation. OpenLane provides automation and optimization tools across various stages of the IC design process, including synthesis, placement, and routing, with the goal of enhancing efficiency and streamlining development cycles. Skywater 130nm process technology, on the other hand, offers a robust platform for semiconductor manufacturing, enabling the fabrication of ICs with competitive performance and power characteristics. Together, these technologies facilitate the creation of advanced ICs with improved performance, power efficiency, and manufacturability



## Design Flow

![124007394-f62a0d80-d9f8-11eb-9d46-7cc885c0eff6](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/5545f843-7521-4500-a54e-26bf51e20f40)


we use the following path of the folder for the flow

`/Desktop/work/tools/openlane_working_dir/openlane`

Then Docker daemon service

`docker`


For step by step flow use interactive mode

`./flow.tcl -interactive`

![Screenshot from 2024-03-30 18-28-46](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/747bfd76-ed46-4e42-b203-4002f3dd3cd7)

importing all the packages required for running the flow using command

`package require for openlane 0.9`

Let's initiate the design preparation phase. OpenLane comes equipped with several default designs. For our workshop, we'll focus on the picorv32a RISC-V Processor. To set up this design in OpenLane, execute the following command.

`prep -design picorv32a`

![Screenshot from 2024-03-30 18-50-24](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/13ce5391-686a-4179-93ec-80ca37011a48)

Next step is Run synthesis

`run_synthesis`

It executes YOSYS and ABC synthesis concurrently, with a processing time ranging from 2 to 3 minutes. Upon completion, it provides a confirmation of successful synthesis. Additionally, it furnishes a wealth of processor-related data for deeper analysis.

![picorv32a STA synthesis+abc](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/35dc35de-b70d-46de-be19-7a4fb3559b28)


As mentioned earlier, the objective for 1st TASK involves computing the flop ratio for D Flip-Flops, a crucial metric for assessing processor performance. This ratio represents the number of D Flip-Flops divided by the total number of cells.

![Flop ratio picorv32a](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/d43c07d2-d741-4d09-8e2c-4dfba685d9c6)

In the above statistics
No. of D Flip Flop is 1613

No. of total cells is 14876

thus the Flop Ratio will be 10.84%

## FLOORPLANING

In the world of chip design, a netlist acts like a map, connecting all the parts of the circuit together. Imagine the chip as a kingdom ruled by two main parts: the Core and the Die. The Core sits inside the Die and holds all the important logic parts of the chip.

Now, let's talk about making the chip as efficient as possible. The Utilization Factor is like a measure of how well space is used. It's the ratio of how much space the blocks take up compared to the total available space.

Think of the Aspect Ratio as how tall and wide the chip is. If it's a perfect square, it has an Aspect Ratio of 1, meaning it's as tall as it is wide.

When it comes to organizing all the blocks on the chip, that's called Floorplanning. It's like arranging furniture in a room, but with computer parts instead.

As we start of our lab work, we'll focus on Floorplanning, which is the next big step after putting together all the parts. Let's shape our designs carefully, setting the stage for amazing technology ahead.

Command to run floorplan

`run_floorplan`

To view the layout in Magic, we must switch to the directory where our floorplan results reside. Let's navigate to that directory now.

`Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/21-03_05-02/results/floorplan/`

`magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &`

![floorplan1](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/4a37e37a-4bbe-456c-987c-8bd4bce94a94)

IO pins are equiplaced

![IO Pins equally placed](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/1a4e97da-79b8-4dbb-86ac-ba8b03e29b66)

VMETAL & HMETAL 

![VMETAL HMETAL1](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/952f8ab5-5046-41a7-896b-6d7b00b5e460)

![floorplan HMETAL](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/90efd74d-ab12-42e6-ae2b-1b8178c9ffd0)

## PLACEMENT
After floorplan we will run placement which aim is to reduce half parameter wire length (HPWL)

`run_placement`

To view placement command is-

`magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech read ../../tmp/merged.lef def read picorv32a.placement.def`

![placement 1](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/fe9db330-21f2-4cc8-8e32-abe5b9065e7c)

![placement 2](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/31a34b30-41e2-458c-b352-c15c99d4fd19)

Please note: Typically, generating the power distribution network (PDN) is included in the floorplan phase. However, in the openLANE flow, the floorplan step does not include PDN generation. The sequence of steps is as follows: floorplan, followed by placement, clock tree synthesis (CTS), and then PDN generation. 

## Standard Cell Design Flow

git clone -

![git clone](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/03db51e9-9e25-45fa-9a5a-668c154ae37c)

![layout](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/3e7c6778-ecab-49ea-b4c1-fca8159bb167)


We will try to extract the Spice netlist from the Inverter using tkcon

![extract files](https://github.com/avinashjaiswal1598/Advanced-Physical-Design/assets/160040323/bca928ae-a798-4241-84af-28bbae5a16e3)
