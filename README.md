<img src="Figures/Cover_1.png" alt="Preview" width="900"/>

# QCD Box
This repository contains the **Design** and **Simulation** files of an 8-port sample box designed for 10x10 mm chips.

## Update:
* **20240919** Published V1.0 

## Description
### Box design in SOLIDWORKS<sup>TM</sup>
* **BOX.SLDASM** is the final assembly of the sample box.
* **BOX_A.SLDASM** and **BOX_B.SLDASM** are the cavity and the lid, respectively.
* **PCB.SLDASM** is the PCB with GCPW-type transmission lines. <br>
(This file is mainly for consistency check. The actual PCB design files should be found in the **PCB** folder.)
* **Chip.SLDASM** is a fake 10x10 mm chip for consistency check.
* **SMA.SLDASM** is a fake SMA connector for consistency check.

\* The **Technical** subfolder contains the .IGS, .SLDDRW, and .pdf files for production.

### PCB design in KiCAD<sup>TM</sup>
* **PCB_240916.gds** is the GDSII file designed by a homemade Python<sup>TM</sup> package (pyads). <br>
(pyads is still in the debugging phase and not available to the public at the current time.)
* **gerber** subfolder contains the gerber files genereted by Keysight ADS<sup>TM</sup>.
* The rest of the files are generated by KiCAD<sup>TM</sup>.

\* The **.zip** file is for production.

### Box simulation in COMSOL<sup>TM</sup>
* **COMSOL_Eigenmode_Box.mph** simulates the bare box mode. The lowest eigenfrequency is found at 12.17 GHz.
* **COMSOL_Eigenmode_Full.mph** simulates the box mode with PCB, chip, and wire bonds.

\* The **Geometry** subfolder contains the simplified geometry for simulation.

### PCB simulation in Keysight ADS<sup>TM</sup>
* **QCDBox_wrk.zip** contains all the simulation files.

## Box design 
### Cavity
Bottom (front side)| Bottom (back side)
:---------:|:---------:
<img src="Figures/bottom_1.png" width="450"/>  |  <img src="Figures/bottom_2.png" width="450"/>

Given that the sample box will normally work in the 100MHz -- 8GHz range, we should minimize the box size to push the first box mode as high as possible. The flange size of the SMA connectors (12.9mm) determines the heightness of the box as 13 mm, as well as the thickness of the side wall as 4 mm. We also add r=3mm fillet for the box.

The inner width of the box is chosen as 21 mm, which will leave a space of 5.5 mm for wire bounding, assuming that an 10x10 mm chip is used. This space should be enough considering that the heightness of the side walls is around 6.5mm. We also add r=2mm fillet for the cavity.

Top (front side)| Top (back side)
:---------:|:---------:
<img src="Figures/top_1.png" width="450"/>  |  <img src="Figures/top_2.png" width="450"/>

We observe from the simulation that it is useful to have an empty space in the lid to push the first box mode to an even higher frequency. The optimal size of the empty space should be 2/3 of the cavity size, saying 14x14 mm, while the height is not crucial (we choose 3.3 mm). We also add r=2mm fillet for the empty space.

We also designed an empty space below the chip to push the bottom ground further way from the chip. The size is 9.5x9.5x3.5 mm in our design, leaving an 0.25 mm distance from the edge to mechanically support the chip. Besides, the chip is supported at the corners by the r=2mm fillet of this empty space.

### SMA connectors
To achieve the best impedance match between the SMA connectors and the PCB with minimum efforts, we choose the panel-mount connectors with flat pin. There are only several products of this type available in the market. We choose the M54FM0112F07C model from Micro RF Connector. The extruded pin is 0.8mm-wide and 1.5mm-long, which will determine the launch pad design of the PCB.

The connector has an 0.8mm-diameter 4mm-long impedance-matched extension that can be inserted into the box. They are mounted to the box through 16 M2x4 screws in total. 

\* The center conductor is gold plated BeCu, which might be slightly magnetic because of Be and Ni in the ENIG process. It is also possible to customize the materials by contacting the company.

### Mounting holes
The box and the lid is assembled with 9 M3x6 screws that are evenly spaced along the edges with an 3mm offset. Owing to the compact size, they are designed as through holes that can be used for mounting the sample box to the cryostat from the back side. 

In addition, we designed 4 addition M3 threads with 3.5mm depth from the back side to faciliate the sample mounting. A dedicated mounting plate can be used if necessary.

The PCB is mounted inside the cavity via 4 M2x3 screws. Brass may be a good choice of these screws as they provide strength as well as electrical and thermal condictivities among the surfaces of the PCB and the bulk cavity.

\* Silver paste may be used to improve the electrical and thermal conductivity.

## PCB design 
### GCPW design
We choose ROGERS<sup>TM</sup> 4350B as the PCB material which has a typical relative dielectric constant of 3.66. According to ROGERS<sup>TM</sup> online MWI Calculator, the dielectric constant is 3.717 in the "wideband" setting when choosing the thickness as 0.508mm. At specific frequencies, such as 100MHz and 10GHz, the dielectric constant varies from 3.8 to 3.72, respectively. In this design, we choose the dielectric constant at 1GHz as our reference, which is 3.758.

The 0.508mm thickness is chosen for several considerations: The major reason is that a smaller thickness can hardly support the 1mm track width as requested by the SMA connector, assuming that GCPW structure is used. A larger thickness solves this problem, but will lead to a larger geometry that is not very convenient for the chip launcher (a=300um, b=180um). The 0.508mm thickness is a sweet point we found that fulfils our needs. In addition, it is also comparable to the thickness of the (675um).

Impedance (a=1.00 mm, b=0.69 mm)|Impedance 2 (a=0.56 mm, b=0.10 mm)
:---------:|:---------:
<img src="Figures/Rogers_4350B_1.png" width="450"/>  |  <img src="Figures/Rogers_4350B_2.png" width="450"/>

Using the ROGERS<sup>TM</sup> online MWI Calculator, we find two parameter settings that provide 50Ohm match. Close to the outer side of the PCB, we choose a=1.00mm and b=0.69mm for soldering the SMA connectors. We change the design to a=0.56mm and b=0.10mm after 1.5mm from the edge, where an additional 0.65mm taper is designed to connect the two geometries.

Although a bit skeptical on whether we need vias for such a short waveguide length, the simulation indicates a meaningful improvement of the scattering parameters with them. We choose the via diemeter as 0.2mm. The vias are 5mm offset from the outer edge of the waveguide slot, and they are evenly spaced by 8mm. In addition, we add vias on the horizontal, vertical, diagonal, and anti-diagonal lines to separate the 8 waveguides.

PCB (front side)|PCB (back side)
:---------:|:---------:
<img src="Figures/PCB_1.png" width="450"/>  |  <img src="Figures/PCB_2.png" width="450"/>

The layout is written by a homemade Python<sup>TM</sup> package, pyads. It generates the GDSII file that can be converted to the gerber files for PCB production. We find it useful to import the generated files to KiCAD<sup>TM</sup> for a 3D overview. Another benefit is that it can be directly linked to multiple online PCB producers to make the order.

### PCB specification
We choose 2-layer PCB with 1oz copper (35um). The vias are not covered but with with 35um thickness of copper plating. We choose immersion sliver for surface finish because the standard process of gold plating would require Ni plating beforehand. 

We find in the simulation that metalization on the inner edges of PCB will significantly suppress the influence of PCB and chip on the box mode. We truncate the waveguide 0.1mm from the inner edges and 0.2mm from the outer edges to faciliate edge plating. The 4 screw holes of 2.2mm diameter are also edge plated.

## Box modes
The box-mode simulations are performed by using the Eigenmode solver of COMSOL<sup>TM</sup>. We search for the eigenmodes around 5GHz with normal mesh size. The perfect electric conductor (PEC) condition is applied to the inner surface of the cavity, all the surfaces of PCB, and the upper surface of the chip. In addition, we define 5 wirebonds on each edge of the chip.<br/><br/>

<img src="Figures/COMSOL_Box.png" width="900"/>
<em>Fig. 1 Bare box modes. Note that the box is upside down.</em><br/><br/>
We first simulate the bare box modes without the PCB and the chip, as shown in Fig. 1. The first 3 modes are observed at 11.524GHz, 15.838/15.841GHz, and 18.845/18.847GHz. This result indicates that the 21mmx21mm is suitable for our needs of operating the box in the 100MHz-8GHz range.<br/><br/>

<img src="Figures/COMSOL_Full.png" width="900"/>
<em>Fig. 2 Box modes with PCB, chip, and wirebonds. Note that the box is upside down</em><br/><br/>
Next, we add the PCB, chip, and the wirebonds for simulation. The simulation results are summarized in Fig. 2. We find that the 1st mode is pulled down to 11.303GHz due to the injection of dielectric materials of the PCB and the chip. The next modes are observed at 11.910/11.912GHz, 14.551GHz, 14.665/14.667/14.669GHz, and 16.226/16.228GHz. Here, the first 2 modes are localized to the chip, the 3rd one is approximately the PCB mode, while the last one is reminiscent to the 2nd bare box mode.<br/><br/>

<img src="Figures/COMSOL_Float.png" width="900"/>
<em>Fig. 3 Box modes with floating chip. The upper row has widebonds but not edge plating, while the low row has edge plating without wirebonds. Note that the box is upside down</em><br/><br/>
We find in the simulation that the edge plating of the PCB is important to prevent a substential drop of the 1st box mode in the full simulation. Figure 3 compares the cases with only wirebonds and only edge plating. In both cases, the 1st box mode drops significantly to approximately 7.5GHz. The field profile suggests different mechanisms for this drop: The mode spreads over the PCB in the former case, while it is mostly concentrated in the gap between PCB and the chip in the latter case. This observation lead to our request of edge plating when producing the PCB.

## PCB characteristics
<img src="Figures/ADS_1.png" width="900"/>
<em>Fig. 4 Mesh and port definition of PCB simulation.</em><br/><br/>
The PCB simulations are performed by using the Momentum solver of Keysight ADS<sup>TM</sup>. We define the WN port as the inout port, and the rest 4 as output ports. The pin edge on the center conductor is set as 0.1mm, while it is 0.5mm on the surface ground.

We simulate the scattering coefficients of the WN waveguide, and its crosstalk to the nearest and next nearest ports. We perform the adaptive frequency sweep from 100MHz to 10GHz. The mesh is defined at the highest simulation frequency with a density of 20 per wavelength. The automatic edge mesh is enabled.

<img src="Figures/ADS_1.png" width="900"/>
<em>Fig. 5 Mesh and port definition of PCB simulation.</em><br/><br/>
Figure 5 summarizes the simulation results.

The transmission loss is kept below 1 dB, while the crosstalk between different ports is less than 30 dB. However, the impedance seems to be 10 $\Omega$ mismatched at 10 GHz (should debug it).

## Mounting procedure:
<img src="Figures/Assembly_1.png" width="900"/>

Spread a very thin layer of silver paste on the backside of PCB.

Place PCB inside the position, align it in the center, and loosely mount it with the 4 M2x3 screws.

Spread the solder paste onto the waveguide tracks.

Insert the SMA connectors into the holes on the side wall and tighten them with M2x4 screws. 

Adjust the PCB so that the pin matches the track, and then tighten it M2x3 screws.

Tough each pin with solder ion to melt the solder paste.

Use multimeter to check DC connection between the SMA pin and the waveguide.

## Acknowledgement:
We are delighted if you find this project helpful to your own study. Feel free to contact us if you have questions, suggestions, criticisms, etc. You are free to copy, share, and build on this project without notifying the authors. 
