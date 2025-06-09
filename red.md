```
aaaa
```
$$
\sum_{i=0}^{10}x_i
$$
New project
![image](./v1749020029.png)
![image](./v1749020050.png)
add project name and directory
![image](./v1749020092.png)
Choose project type
![image](./v1749020100.png)
Add source (Skip)
![image](./v1749020160.png)
Add constraints
![image](./v1749020212.png)
![image](./v1749020194.png)
Choose board
![image](./v1749020241.png)
The Vivado window for the project
![image](./v1749020294.png)

Create a block design
![image](./v1749020294.png)
![image](./v1749020999.png)
![image](./v1749021013.png)

Right-click on the blank design and select Add IP to instantiate:

ZYNQ 7 Processing System

AXI GPIO (we will use this IP to write from PS to PL)

![image](./v1749021475.png)
![image](./v1749021504.png)
![image](./v1749021529.png)
![image](./v1749021550.png)
![image](./v1749021581.png)
![image](./v1749021601.png)


Run block automation

![image](./v1749023010.png)
![image](./v1749023034.png)
![image](./v1749023063.png)
![image](./v1749023114.png)


After creating a new project, we have to add the IP repository that has been created to simplify the use of analog frontends of the Redpitaya-125-14:

Go to the left panel click on Project Manager --> Settings.
Navigate to IP --> Repository.
Add the repository FPGA-Notes-for-Scientists/ip.


![image](./v1749024314.png)
![image](./v1749024337.png)
![image](./v1749024348.png)


Create an instance of the following IPs:

ZYNQ7 Processing System
Processor System Reset
AXI GPIO
AXI Interconnect (x2)
AXI Direct Memory Access (x2)
Redpitaya-125-14-clk
Redpitaya-125-14-adc
Redpitaya-125-14-dac

![image](./v1749024132.png)
![image](./v1749024178.png)

Double click on the ZYNQ7 instance and go to PS-PL Configuration --> HP Slave AXI Interface and enable the high performance interfaces HP0 and HP1 (data width 64 bit).

![image](./v1749105741.png)
![image](./v1749105829.png)
![image](./v1749105878.png)
![image](./v1749105991.png)
![image](./v1749106024.png)

Connect all clocks and resets
![image](./v1749106716.png)

## Connect all ports

- Edit the constraints in redpitaya-125-14.xdc and uncomment the sections:

ADC (lines 8-56)

DAC (lines 59-90)

Clock constraints (lines 180-183)

- Go back to your design and create the required input/output ports for the Redpitaya-125-14-clk, Redpitaya-125-14-adc and Redpitaya-125-14-dac IPs. The fastest way is to right-click on a every port and choose Create Port (CTRL + K) and proceed with the default settings.

- Including all the required ports, the design becomes:

![image](./v1749107623.png)
![image](./v1749108103.png)
![image](./v1749108841.png)





Connect AXI_interconnect to the processor HP port

![image](./v1749109005.png)
![image](./v1749109097.png)


add stream control to the project

16bit

32bit

![image](./v1749109308.png)
![image](./v1749109414.png)


add stream modules into the project

16 Bit

![image](./v1749109520.png)
![image](./v1749109525.png)


32 Bit

![image](./v1749109570.png)


Connect stream control to ADC and connect clk and reset
![image](./v1749109865.png)

We need a way to set parameter in the PL. We will use register bank

we need one for triger and one for numner of samples in stream control. 

![image](./v1749110262.png)



then we connect the register outoput to both stream controls
![image](./v1749110560.png)


Validate Design

![image](./v1749189342.png)
![image](./v1749189315.png)

There are 3 unconnected ports. Two are in axi_dma and another one is DAC.

Firat, we creat HDL Wrapper
![image](./v1749190885.png)
![image](./v1749190894.png)
![image](./v1749190985.png)



we need data for DAC. We will use DDS with some parameters. In order to include DDS into the project we will use DDS wrapper. 

First, add these two source files

![image](./v1749191392.png)

We observe that the dds ip is in lock status. We need to upgrade this using `Report IP Status`
![image](./v1749191573.png)


![image](./v1749191598.png)

![image](./v1749191627.png)

Add DDS wrapper to the project


![image](./v1749191711.png)


We observe that the DDS wrapper need to parameters. We need to add therse parameters into the register so that we can set them from python script




We can now connect them to the DDS Wrapper and connect the clock as well.


We then connect the DDS Wrapper output to DAC,



Verify the connection



Add FFT and set the FFT IP

Set Target frequency and bit rate and Transform length to 8192

Set Natural order and add reset




Click Run connection automation



Click run connecttion automation




