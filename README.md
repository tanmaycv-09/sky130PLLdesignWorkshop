# Phase-Locked Loop(PLL) IC design on Open-Source Google-Skywater 130nm Workshop

<p align="center"><a href="https://www.vlsisystemdesign.com/pll-design-using-sky130/"><img width="833" alt="Screenshot 2022-03-06 at 9 00 55 AM" src="https://user-images.githubusercontent.com/77117825/156908010-9d44ba40-339d-4047-97e7-cbef360f11a5.png"></a></p>

# Brief Description of the Workshop
  
 This workshop was of 2 days conducted by VLSI System Design on the topic Phase-Locked Loop(PLL) IC design on Open-Source Google-Skywater 130nm. Over the course of two days this workshop explains the PLL inside out and we also have to design a PLL using certain simulation softwares (ngspice and magic). On the first day of the workshop, we are introduced to PLL and we discuss the circuitry required to design one. We discuss in depth about the individual components used and their role in operating the PLL. We also install the softwares on the first day and this concludes the first day. On the second day of the workshop, we start by preparing spice files and conducting spice simulations of individual components of the PLL. Then using these components, we create a spice file that will work effectively as a complete PLL. After this, we look into the layouts of the individual components and finally combine them to form a final PLL layout. We perform post-layout simulations on the individual components and end the day with discussion on tapeouts and Caravel.

  # Table of Contents 
   - [Day1 PLL Theory and Lab Setup](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/blob/main/README.md#day1-pll-theory-and-lab-setup)
     - [Part-1 Introduction to PLL](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-1-introduction-to-pll)
     - [Part 2: Introduction to Phase Frequency Detector](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-2-introduction-to-phase-frequency-detector)
     - [Part 3: Introduction to Charge Pump](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-3-introduction-to-charge-pump)
     - [Part 4: Introduction to Voltage Controlled Oscillator and Frequency Divider](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-4-introduction-to-voltage-controlled-oscillator-and-frequency-divider)
     - [Part 5: Tool Setup and Design Flow](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-5-tool-setup-and-design-flow)
     - [Part 6: Introduction to PDK, specifications and pre-layout circuits](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-6-introduction-to-pdk-specifications-and-pre-layout-circuits)
     - [Part 7: Circuit design simulation tool - Ngspice Setup](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-7-circuit-design-simulation-tool---ngspice-setup)
     - [Part 8: Layout design tool - Magic Setup](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-8-layout-design-tool---magic-setup)
   - [Day 2: PLL Labs and post-layout simulations](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#day-2-pll-labs-and-post-layout-simulations)
     - [Part - 9  PLL components circuit simulations](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part---9--pll-components-circuit-simulations)
     - [Part 10: Steps to combine PLL sub-circuits and PLL full design simulation](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-10-steps-to-combine-pll-sub-circuits-and-pll-full-design-simulation)
     - [Part 11: Troubleshooting steps](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-11-troubleshooting-steps)
     - [Part 12: Layout design](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-12-layout-design)
     - [Part 13: Layout Walkthrough](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-13-layout-walkthrough)
     - [Part 14: Parasitic Extraction ](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-14-parasitic-extraction)
     - [Part 15: Post Layout simulations](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#part-15-post-layout-simulations)
     - [Conclusion, Opinion & Challenges ](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#conclusion-opinion--challenges)
     - [References](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/edit/main/README.md#references)

  # Day1 PLL Theory and Lab Setup
On the first day of the workshop, basic theory of the PLL was taught. The multiple components that the PLL constitutes were discussed in depth and their MOSFET based circuits were seen. The tools that were to be installed were - Ngspice and Magic. The steps for the installations were told and a walkthrough video was presented if any difficulties were faced. The MOSFET based circuits that were taken were chosen such that they were up-to-date with the latest advancements and minimal disadvantages.
  
 # Part-1 Introduction to PLL
  - To get a precise clock signal without frequency or phase noise and at the same time it has the flexibility of running at a frequency of our choice.
  - Two ways in which clock signal can be generated for our application:
    - Quartz crystals
    - Voltage Controlled Oscillators
  - Quartz Crysatals have a pure spectrum (there aren't any unwanted frequencies). However, it is designed to give only a certain frequency output and they aren't flexible like the Voltage Controlled Oscillators.
  - Voltage Controlled Oscillator can be implemented on chip easily with inverters and the frequency can be controlled with an input voltage. However, they tend to have noise or fluctuations in their phase.
- The purpose of PLL is to make the spectrum of a Voltage Controlled Oscillator pure while still maintaining the flexibility.
  
  ![WhatsApp Image 2022-03-06 at 10 18 19 AM](https://user-images.githubusercontent.com/77117825/156909660-7e550073-1e4f-45e3-b222-27c75e54b99c.jpeg)
- Phase-Locked Loop Intuition
   - To mimic the reference means to have the same/a multiple of the reference frequency and a constant phase difference with it.
  
 ![WhatsApp Image 2022-03-06 at 10 25 04 AM](https://user-images.githubusercontent.com/77117825/156909823-98bcc13c-6168-4171-8435-2cdb65c5aa6b.jpeg)

 - Components of a basic PLL are 
   - VCO (Voltage Controlled Oscillator): It is the on-chip oscillator.
   - PFD (Phase Frequency Detector): It takes care of the comparison of the output signal and the reference signal.
   - CP (Charge Pump): It converts the digital comparison output of the PFD into an analog signal that converts the frequency of the VCO
   - LPF (Low Pass Filter): It serves to smoothen the CP output signal. It also has a much bigger role in sustainability of the feedback load.
   - FD (Frequency Divider): It converts the whole system into a frequency multiplier.
   - Such a PLL is called as a Clock Multiplier PLL.
  
  # Part 2: Introduction to Phase Frequency Detector
  - When we use the XOR function on the reference signal and the output signal, if the phase increases then the width of the pulse of the XOR signal also increases.
  - So the width of the XOR pulse can be used as a measure of the phase difference.
  - XOR operation doesn't distinguish between which signal is leading and lagging.
  - The ability to distinguish between lagging and leading signal is essential for designing a PLL.
  - This can be achieved by using two signals - Up and Down signals
  - Down signal is activated if the falling edge of the output signal is obtained before the falling edge of the reference signal and it stays active until the falling edge of the reference signal is received.
  - UP signal is activated if the falling edge of the reference signal is obtained before the falling edge of the output signal and it stays active until the falling edge of the output signal is received.
  - This can be depicted in the state machine format as follows:
  
  <p align="center"><img src="https://user-images.githubusercontent.com/77117825/156910203-73a12fc6-7da6-4608-bde7-4781203e4e3d.jpeg"></p>

  - A good way to detect falling or rising edge is by usng a flip flop
  - Implementing the circuit with flip flops
    - The negative edge flip flops are used here. In this case, it is known that if the clock input falls, the flip flop output will go high. This indicates the falling edge.
    - Two flip flops are required because of the need to detect the falling edge of two different signals in different scenarios.
    - Now, from the state diagram, the falling edge of the reference signal arrives and then the falling edge of the output signal arrives. Both up and down signals should now become low. To get this, an additional AND gate is required.

  ![WhatsApp Image 2022-03-06 at 10 42 26 AM](https://user-images.githubusercontent.com/77117825/156910209-58894511-328a-4590-b479-acc735624896.jpeg)
  - This is one of the boost phase detector circuits.
  - There is and issue with the circuit - Dead Zone:
     - Dead Zone prevents us from improving the smallest difference in phase or frequency that the PFD is able to measure.
     - If the signals are very close, then we get an output which is clipped as there isn't enough time for it to rise.
     - A more precise PFD enables better stability for the PLL because it enables minute adjustments of the phase and frequency.
  
  # Part 3: Introduction to Charge Pump
  
  - The role of a CP in PLL is to convert the difference in phase or frequency which is measured digitally into an analog signal that can be used to control the VCO.
  - It can be done by using current steering circuits. A current steering circuit steers the current or it directs the current flow from the Vdd to the output or from the output to the ground based on the up or down signal that is provided.
  - If up signal is active, the current flows from the Vdd to the output capacitor and charges the output. This increases the voltage at the CP output.
  - If down signal is active, the current flows from the output capacitor to the ground and discharges the output. This decreases the voltage at the CP output.

  ![WhatsApp Image 2022-03-06 at 10 50 31 AM](https://user-images.githubusercontent.com/77117825/156910342-c41fd09f-c5fe-4e4a-94a9-35eaca3eec1d.jpeg)
 - The reluctance of the capacitor to change voltage quickly, smoothens or averages the voltage change at the output so when the average active time of the up signal is higher than the down signal, the output voltage rises.
 - Similarly, when average active time of down signal is higher than the up signal, the output voltage falls.
 - Increasing the voltage speeds up the VCO while reduction involtage slows it down.
 
 ![WhatsApp Image 2022-03-06 at 10 53 01 AM](https://user-images.githubusercontent.com/77117825/156910402-3d0a2175-5a74-4320-8968-cc858bb514ad.jpeg)

 - When the up and down transistors are off, there is still a small current flowing through them in the form of leakage current. This leakage current has a bad impact on output control voltage because it keeps charging the output capacitor even when there is no up or down signal.
 - The averaging is still not as smooth as required. There are still fluctuations caused according to the rise and fall of the up and down signals. We can tackle this by replacing the output capacitor with a low pass filter.
 - This smoothens out any high frequency fluctuations in the output. LPF has a very significant role in stabilizing the PLL. Without the load filter, the PLL cannot lock and mimic the reference signal.
 - There are two thumb rules that one should remember:
 - Cx ~= C/10
 - Loop filter band width ~= (Higher output frequency)/10
  
# Part 4: Introduction to Voltage Controlled Oscillator and Frequency Divider
  
 -  Ring Oscillator:
    - It is a sries combination of odd number of inverters which has a certain delay. This causes the output signal to flip after a certain delay.
    - Output period = 2 * delay(inverter) * inverter_count
 - The frequency depends on delay and delay depends on the current supplied.
 - If the current is high, then the output can be charged faster.
 - By 'starving' the ring oscillator of current, we can control its frequency.
  
 <p align="center"><img src="https://user-images.githubusercontent.com/77117825/156910502-9c8b8596-d127-48f1-aec1-432c07775164.jpeg"></p>

 
  ![WhatsApp Image 2022-03-06 at 11 01 39 AM](https://user-images.githubusercontent.com/77117825/156910618-a7d7dd1a-aa00-46ef-82b2-6c42715ad824.jpeg)

 - Frequency divider:
    - The output of a toggle flip flop is half the frequency of the input signal.
  
  ![WhatsApp Image 2022-03-06 at 11 06 15 AM](https://user-images.githubusercontent.com/77117825/156910709-28f122ee-b4f4-4f19-95d1-b37c21f7e32d.jpeg)

  - Some Terms:
     - Lock Range: It is the range of frequencies for which the PLL is able to maintain a lock given that it is already in a locked state.
     - Capture Range: It is the range of frequencies for which the PLL is is able to attain a locked stated from an unlocked stated.
     - Settling Time: It is the time within which the PLL is able to attain a lock from an unlocked state.
  
 # Part 5: Tool Setup and Design Flow
  
  - Best to build any software tool from its source code, as it will be the latest version.

  - Tools used:

     - Ngspice: It is used for the transistor level circuit simulation
     - Magic: It is used for layout design and parasitic extraction
   - In order to install Ngspice, type the following command in the terminal:
         - sudo apt-get install ngspice
   - In order to install Magic, follow the given steps:
       - sudo apt-get update && sudo apt-get upgrade This step is used to update the OS.
       - git clone git://opencircuitdesign.com/magic This step clones the Magic Repository
       - sudo apt-get install csh This step installs the csh shell
       - cd magic This step is to go into the cloned directory
       - ./configure This step runs the configure script
       - make This step runs the make command to compile
       - sudo make install This step installs magic on the device
  
    - Development Flow:
  
 <p align="center"> <img width="244" alt="Screenshot 2022-03-06 at 11 15 09 AM" src="https://user-images.githubusercontent.com/77117825/156910939-20d2d822-7c0f-4f82-911c-59f7d8381180.png"></p>
  
   - It is often the case that after each simulation phase, we would need to make several changes to the circuit to bring it closer to the required specifications.

  # Part 6: Introduction to PDK, specifications and pre-layout circuits
   
  - The PDK (Process Design Kit) Content:
    - io: input-output
    - pr: primitives (spice)
    - sc: standard cell
    - hd: high density
    - hs: high speed
    - lp: low power
    - hdll: high density low leakage
  - PDK is provided by the fabrication centres because thats where the transistors get fabricated.
  - The characteristics of those transistors in the technology node of interest are available to us through the scripts.
  - Other than transistor characteristics, a lot of information is available to help the design process.
  - The specifications give the operationg condition at which the PLL has to operate.
  - It is based on these specifications, that we will design the circuit.
  - We will use the simplified IP specifications from VSD for our PLL design:
    - Corner - TT
    - Supply - 1.8V
    - Room Temperature
    - VCO mode and PLL mode
    - Input Fmin = 5MHz and Fmax = 12.5MHz
    - Multiplier - 8x
    - Jitter (RMS) <~ 20ns
    - Duty Cycle - 50%
                      
  - The first three specifications together are called as PVT corner or Process-Voltage-Temperature corner
  - Pre-layout:
    - This phase is all about development and the transistor level simulation of the circuits.
    - In this phase all the circuits are developed in such a way that most of the disadvantages are already overcome.     
                      
  # Part 7: Circuit design simulation tool - Ngspice Setup
    - The first step is to install ngspice using ubuntus package manager.
    - Next, we have to clone the google skywater pdk. On the terminal type git clone https://github.com/google/skywater-pdk-libs-sky130_fd_pr.git
    - Now we have to pick the files that we need from "skywater-pdk-libs-sky130_fd_pr" folder
    - Go to the cells folder and search nfet and in nfet folder search "nfet_01v8". Again, search "tt" and chose the file named "sky130_fd_pr__nfet_01v8__tt_leak.pm3.spice". Copy this file to the directory that will be used for PLL simulations.
    - Go to the cells folder and search pfet and in nfet folder search "pfet_01v8". Again, search "tt" and chose the file named "sky130_fd_pr__pfet_01v8__tt_leak.pm3.spice". Copy this file to the directory that will be used for PLL simulations.
    - Go to models and then go to parameters and copy the files "invariant.spice" and "lod.spice" to the directory that will be used for PLL simulations.
    - Now in terminal go to the directory that will be used for PLL simulations.
    - In the terminal type the command nano sky130.lib.
    - Now, include all the files that were just copied.
    - Save this file using ctrl+s and then exit using ctrl+x.
    
  # Part 8: Layout design tool - Magic Setup
                      
 - The first step is to clone the magic repository (given in Part 5)
 - Now we have to install the dependancies which can be found at the [Install](http://opencircuitdesign.com/magic/) page
 - Now go into the magic folder using the "cd" command and compile magic using ./configure command.
 - Run the configure, make and install commands on the terminal.
 - Search open pdks on google and select the [RTimothy/open_pdks](https://github.com/RTimothyEdwards/open_pdks)
 - Clone this repository and compile it or download [sky130A.tech](https://drive.google.com/file/d/18BG4zzpRrcHP0UoBcNLA3sWFDVdNlUtp/view) and place it in the main folder.
 - **This is the end of the tasks of the first day.

# Day 2: PLL Labs and post-layout simulations
                      
 On the second day of the workshop, we did a lot of things. Firstly, we performed pre-layout simulations for all the components individually. Then we combined all the files and did a pre-layout spice simulation for the complete PLL. Then we were given a walkthrough of the layout of the PLL. We performed post-layout simulations for the components at this stage and then say the complete layout of the PLL. We created the GDS file for our layout and also saw the further steps required for the tapeout process.                     
                      
 - We created a circuit description ngspice and we did it for the frequency divider circuit.
 - A spice file is just a text file with a .spice or .cir extension.
 - To make the file type the following command in the terminal - touch FreqDiv.cir.                      
                      
 # Part - 9  PLL components circuit simulations
  - Frequency Divider Circuit

        - *FD
          .include spice_lib/sky130.lib

         xm1 1 2 3 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
         xm2 0 2 3 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

         xm3 3 Clkb 4 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
         xm4 3 Clk 4 0 sky130_fd_pr__nfet_01v8 l=150n w=840n 

         xm7 1 4 5 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
         xm8 0 4 5 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

         xm9 5 Clk 6 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
         xm10 5 Clkb 6 0 sky130_fd_pr__nfet_01v8 l=150n w=640n 

         xm11 1 6 2 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
         xm12 0 6 2 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

         xm13 1 Clk Clkb 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
         xm14 0 Clk Clkb 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

         v1 1 0 1.8
         v2 Clk 0 PULSE 0 1.8 1n 6p 6p 5ns 10ns

         c1 6 0 10f
         .control
         tran 0.1ns 0.2us
         plot v(6) v(Clk)+2
         .endc

         .end

> Output 

<p align="center"><img width="409" alt="FD waveform" src="https://user-images.githubusercontent.com/77117825/156912838-c077737a-16b2-4261-8706-65a2528bfe86.png"></p>

> Command window result 

     - Initial Transient Solution
     --------------------------
     
     Node                                   Voltage
     ----                                   -------
     1                                          1.8
     2                                      1.79148
     3                                  1.10966e-06
     clkb                                       1.8
     4                                     0.884093
     clk                                          0
     5                                     0.633474
     6                                     0.633474
     v2#branch                                    0
     v1#branch                         -1.32296e-05
     
      Reference value :  0.00000e+00


 - Simulation File for Charge Pump Circuit
   - when both up and down = 0

           - *CP

         .include sky130nm.lib

         xm43 3 2 1 1 sky130_fd_pr__pfet_01v8 l=150n w=5.4u 
         xm44 out downb 3 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
         xm31 out up 7 0 sky130_fd_pr__nfet_01v8 l=150n w=420n
         xm32 7 8 0 0 sky130_fd_pr__nfet_01v8 l=150n w=5.4u

         xm33 2 2 1 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
         xm34 8 8 0 0 sky130_fd_pr__nfet_01v8 l=150n w=420n
         
         xm35 9 down 3 1 sky130_fd_pr__pfet_01v8 l=150n w=5400n 
         xm36 9 9 0 0 sky130_fd_pr__nfet_01v8 l=150n w=420n

         xm37 10 10 1 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
         xm38 10 upb 7 0 sky130_fd_pr__nfet_01v8 l=150n w=5400n

         xm39 1 down downb 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
         xm40 0 down downb 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

         xm41 1 up upb 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
         xm42 0 up upb 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 
         v1 1 0 1.8	
         v2 up 0 0
         *PULSE 0 1.8 1n 6p 6p 100ns 200ns
         v3 down 0 0

         r1 out rc 200
         c1 rc 0 64f
         c2 out 0 10f

         .ic v(out) = 0
         .ic c(out) = 0
         .control
         tran 1ns 1us
         plot v(out) C(out)
         *plot v(6) V(Clk)+2
         * v(D) v(Clk) v(6)
         .endc
         
      > output for up=down=o

    
  <p align="center"><img width="754" alt="CP_up_down_0" src="https://user-images.githubusercontent.com/77117825/156913092-98fa6cfd-0d89-44f4-b715-89f3b4f8efe6.png"></p>
  
  > output of command window

      - Initial Transient Solution
        --------------------------
        
        Node                                   Voltage
        ----                                   -------
        3                                     0.401233
        2                                          1.8
        1                                          1.8
        out                                          0
        downb                                      1.8
        up                                           0
        7                                      1.46925
        8                                  2.47938e-39                                    0.371206
        down                                         0
        10                                     1.73656
        upb                                        1.8
        rc                                           0
        v3#branch                                    0
        v2#branch                                    0
        v1#branch                          -1.9588e-10
        
         Reference value :  0.00000e+00
 
 
   > output when we give Up pulse 
     - v2 up 0 PULSE 0 1.8 1n 6p 6p 100ns 200ns    
       
     - *CP

    .include spice_lib/sky130.lib

    xm43 3 2 1 1 sky130_fd_pr__pfet_01v8 l=150n w=5.4u 
    xm44 out downb 3 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
    xm31 out up 7 0 sky130_fd_pr__nfet_01v8 l=150n w=420n
    xm32 7 8 0 0 sky130_fd_pr__nfet_01v8 l=150n w=5.4u

    xm33 2 2 1 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
    xm34 8 8 0 0 sky130_fd_pr__nfet_01v8 l=150n w=420n

    xm35 9 down 3 1 sky130_fd_pr__pfet_01v8 l=150n w=5400n 
    xm36 9 9 0 0 sky130_fd_pr__nfet_01v8 l=150n w=420n
    
    xm37 10 10 1 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
    xm38 10 upb 7 0 sky130_fd_pr__nfet_01v8 l=150n w=5400n
    
    xm39 1 down downb 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
    xm40 0 down downb 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

    xm41 1 up upb 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
    xm42 0 up upb 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 
    v1 1 0 1.8	
    v2 up 0 PULSE 0 1.8 1n 6p 6p 100ns 200ns
    v3 down 0 0
    
    r1 out rc 200
    c1 rc 0 64f
    c2 out 0 10f

    .ic v(out) = 0
    .ic c(out) = 0
    .control
    tran 1ns 1us
    plot v(out) C(out)
    *plot v(6) V(Clk)+2
    * v(D) v(Clk) v(6)
    .endc
    
    
   > output wave 

 
<p align="center"><img width="1268" alt="CP_up_pulse" src="https://user-images.githubusercontent.com/77117825/156913269-f454fb7f-3e48-4ef0-8c3e-f87a14c5a47f.png"></p>

 - Simulation File for VCO

       - *Current starved 3 stage VCO
       .include spice_lib/sky130.lib

       xm1 10 16 3 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
       xm2 3 16 9 9  sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm3 10 3 4 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
       xm4 4 3 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm5 10 4 12 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
       xm6 12 4 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm11 10 12 13 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
       xm12 13 12 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm13 10 13 14 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
       xm14 14 13 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm15 10 14 15 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
       xm16 15 14 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm17 10 15 16 10 sky130_fd_pr__pfet_01v8 l=150n w=2400n
       xm18 16 15 9 9 sky130_fd_pr__nfet_01v8 l=150n w=1200n

       xm7 10 5 1 1 sky130_fd_pr__pfet_01v8 l=150n w=1080n
       xm8 5 5 1 1 sky130_fd_pr__pfet_01v8 l=150n w=840n
       xm9 5 in 0 0 sky130_fd_pr__nfet_01v8 l=150n w=840n
       xm10 9 in 0 0 sky130_fd_pr__nfet_01v8 l=150n w=1080n

       xm19 1 16 11 1 sky130_fd_pr__pfet_01v8 l=150n w=1080n
       xm20 11 16 0 0 sky130_fd_pr__nfet_01v8 l=150n w=540n

       *c1 11 0 10f
       v1 1 0 1.8
       v2 in 0 0.6


       .control
       tran 0.1ns 0.5us
       plot v(in) v(11)
       *setplot tran1
       *linearize v(14)
       *set specwindow=blackman
       *fft v(14)
       *dc v2 0 1.2 0.01
       *plot mag(v(14))
       .endc
       .end       
       
       
  > output wave 

 <p align="center"><img width="1345" alt="VCO_wave" src="https://user-images.githubusercontent.com/77117825/156913366-360d2aab-f406-456e-820b-56a1778669f6.png"></p>

  - Simulation File for PFD

        - *PD_10T
        .include sky130nm.lib

        xm1 1 clk1 3 1 sky130_fd_pr__pfet_01v8 l=150n w=640n 
        xm2 3 clk1 4 0 sky130_fd_pr__nfet_01v8 l=150n w=1800n
        xm3 4 clk2 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

        xm4 1 clk2 6 1 sky130_fd_pr__pfet_01v8 l=150n w=640n 
        xm5 6 clk2 7 0 sky130_fd_pr__nfet_01v8 l=150n w=1800n
        xm6 7 clk1 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

        xm7 8 clk1 3 0 sky130_fd_pr__nfet_01v8 l=150n w=840n 
        xm8 clk1 clk1 8 1 sky130_fd_pr__pfet_01v8 l=150n w=640n

        xm11 upb 8 1 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
        xm12 upb 8 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

        xm15 up upb 1 1 sky130_fd_pr__pfet_01v8 l=150n w=960n
        xm16 up upb 0 0 sky130_fd_pr__nfet_01v8 l=150n w=480n
  
        xm9 9 clk2 6 0 sky130_fd_pr__nfet_01v8 l=150n w=840n
        xm10 clk2 clk2 9 1 sky130_fd_pr__pfet_01v8 l=150n w=640n

        xm13 downb 9 1 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
        xm14 downb 9 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

        xm17 down downb 1 1 sky130_fd_pr__pfet_01v8 l=150n w=960n
        xm18 down downb 0 0 sky130_fd_pr__nfet_01v8 l=150n w=480n


        *output cap
        c1 up 0 3f
        c2 down 0 3f

        *sources
        v1 1 0 1.8v
        v2 clk1 0 pulse(0 1.8 0 6p 6p 5ns 10ns)
        v3 clk2 0 pulse(0 1.8 6ns 6p 6p 5ns 10ns) 

        *simulation
        .control
        tran 10ns 800ns 120ns
        plot v(clk2)+4 v(clk1)+4 v(up)+2 v(down)
        .endc
        .end

 > output of simulation 

 
<p align="center"><img width="1373" alt="PD_wave" src="https://user-images.githubusercontent.com/77117825/156913676-2d6b3bbb-215d-41e0-b34c-9d8194e00763.png"></p>

> zoom in pic 

<p align="center"><img width="833" alt="Screenshot 2022-03-06 at 1 05 06 PM" src="https://user-images.githubusercontent.com/77117825/156913727-5b4fd63e-a944-4876-8b6b-dfce1a6fa67a.png"></p>

# Part 10: Steps to combine PLL sub-circuits and PLL full design simulation

 > PLL circuit 

     - *PLL
       .include spice_lib/sky130.lib

       xx1 Clk_Ref Clk_Out_by_8 up down pd
       xx2 up down VCtrl cp

       *Loop Filter
       r1 VCtrl rc1 490
       c1 rc1 0 355f
       r2 rc1 rc2 490
       c2 rc2 0 350f
       r3 rc2 rc3 490
       c3 rc3 0 345f

       xx3 rc3 Clk_Out vco

       xx4 Clk_Out Clk_Out_by_2 fd
       xx5 Clk_Out_by_2 Clk_Out_by_4 fd
       xx6 Clk_Out_by_4 Clk_Out_by_8 fd

       v1 Clk_Ref 0 PULSE 0 1.8 0 6ps 6ps 40ns 80ns

       .ic v(VCtrl) = 0
       .ic v(Clk_Out_by_2) = 0
       .ic v(Clk_Out_by_4) = 1.8
       .ic v(Clk_Out_by_8) = 0
       .control
       tran 0.1ns 180us
       plot v(Clk_Ref) v(Clk_Out_by_8) v(Clk_Out_by_4)+2 v(Clk_Out_by_2)+4 v(Clk_Out)+6 v(up)+8 v(down)+10 v(VCtrl)+12
       .endc

       *PFD
       .subckt pd Clk1 Clk2 up down 
       xm1 1 clk1 3 1 sky130_fd_pr__pfet_01v8 l=150n w=640n 
       xm2 3 clk1 4 0 sky130_fd_pr__nfet_01v8 l=150n w=1800n
       xm3 4 clk2 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

       xm4 1 clk2 6 1 sky130_fd_pr__pfet_01v8 l=150n w=640n 
       xm5 6 clk2 7 0 sky130_fd_pr__nfet_01v8 l=150n w=1800n
       xm6 7 clk1 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

       xm7 8 clk1 3 0 sky130_fd_pr__nfet_01v8 l=150n w=2400n 
       xm8 clk1 clk1 8 1 sky130_fd_pr__pfet_01v8 l=150n w=640n

       xm11 upb 8 1 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
       xm12 upb 8 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

       xm15 up upb 1 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
       xm16 up upb 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n
  
       xm9 9 clk2 6 0 sky130_fd_pr__nfet_01v8 l=150n w=2400n
       xm10 clk2 clk2 9 1 sky130_fd_pr__pfet_01v8 l=150n w=640n

       xm13 downb 9 1 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
       xm14 downb 9 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

       xm17 down downb 1 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
       xm18 down downb 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n


       *output cap
       *c1 up 0 6f
       *c2 down 0 6f

       v1 1 0 1.8
       .ends pd


       *CP
       .subckt cp up down out
       xm43 3 2 1 1 sky130_fd_pr__pfet_01v8 l=150n w=18u 
       xm44 out downb 3 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
       xm31 out up 7 0 sky130_fd_pr__nfet_01v8 l=150n w=420n
       xm32 7 8 0 0 sky130_fd_pr__nfet_01v8 l=150n w=4.8u

       xm33 2 2 1 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
       xm34 8 8 0 0 sky130_fd_pr__nfet_01v8 l=150n w=420n
       
       xm35 9 down 3 1 sky130_fd_pr__pfet_01v8 l=150n w=5400n 
       xm36 9 9 0 0 sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm37 10 10 1 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
       xm38 10 upb 7 0 sky130_fd_pr__nfet_01v8 l=150n w=5400n

       xm39 1 down downb 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
       xm40 0 down downb 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

       xm41 1 up upb 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
       xm42 0 up upb 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

       *r1 out rc 200
       *c1 rc 0 8f

       v1 1 0 1.8
       .ends cp


       *VCO
       .subckt vco in 17
       xm1 10 16 3 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
       xm2 3 16 9 9  sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm3 10 3 4 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
       xm4 4 3 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm5 10 4 12 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
       xm6 12 4 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm11 10 12 13 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
       xm12 13 12 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm13 10 13 14 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
       xm14 14 13 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm15 10 14 15 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
       xm16 15 14 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

       xm17 10 15 16 10 sky130_fd_pr__pfet_01v8 l=150n w=2400n
       xm18 16 15 9 9 sky130_fd_pr__nfet_01v8 l=150n w=1200n

       xm7 10 5 1 1 sky130_fd_pr__pfet_01v8 l=150n w=1080n
       xm8 5 5 1 1 sky130_fd_pr__pfet_01v8 l=150n w=840n
       xm9 5 in 0 0 sky130_fd_pr__nfet_01v8 l=150n w=840n
       xm10 9 in 0 0 sky130_fd_pr__nfet_01v8 l=150n w=1080n

       xm19 1 16 11 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
       xm20 11 16 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

       xm21 1 11 17 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
       xm22 17 11 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

       *c1 11 0 24f
       v1 1 0 1.8
       .ends vco


       *FD
       .subckt fd Clk 10

       xm1 1 2 3 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
       xm2 0 2 3 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

       xm3 3 Clkb 4 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
       xm4 3 Clk 4 0 sky130_fd_pr__nfet_01v8 l=150n w=840n 

       xm7 1 4 5 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
       xm8 0 4 5 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

       xm9 5 Clk 6 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
       xm10 5 Clkb 6 0 sky130_fd_pr__nfet_01v8 l=150n w=640n 
       
       xm11 1 6 2 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
       xm12 0 6 2 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

       xm13 1 Clk Clkb 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
       xm14 0 Clk Clkb 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

       xm15 7 6 1 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
       xm16 7 6 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

       xm19 1 7 10 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
       xm20 10 7 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 


       *c1 7 0 18f
       v1 1 0 1.8
       .ends fd       


  > output wave 

<p align = "center"><img width="1295" alt="Screenshot 2022-03-05 at 8 26 37 PM" src="https://user-images.githubusercontent.com/77117825/156913894-3b88c68b-492b-460b-ae0a-ca618a60da25.png"></p>

 > zoomed view 1
<p align="center"><img width="836" alt="Screenshot 2022-03-06 at 1 12 15 PM" src="https://user-images.githubusercontent.com/77117825/156916066-92061bab-00ea-494d-aa6b-e2f7d1389271.png"></p>

> zoomed view 2 ( if we zoom in at the bottom, we can the the difference between the reference signal in red and the output frequency divided by 8 signal as shown below:)
<p align="center"><img width="836" alt="Screenshot 2022-03-06 at 2 26 33 PM" src="https://user-images.githubusercontent.com/77117825/156916128-b74de461-0b0f-4e71-be4f-a0aa5acfb87b.png"></p>

   -  This difference in output signal divided by 8 and the reference signal is the phase noise of this PLL that we created.
   -  If the PLL becomes perfect, which is an ideal case, then the blue feedback signal will overlap the red reference signal perfectly which is what we wish for.
   -  If we take the root-mean-square (RMS) of the variation of the output signal, we get the Jitter (RMS) value which denotes the phase noise.
   -  It is important to note that this blue signal is in-fact created by the VCO.
# Part 11: Troubleshooting steps

  - Observe what kind of issue is faced.
  - Always debug individual components fully before moving to the combined simulation.
  - Check if any signals are coming flat or if the simulation is crashing. If this is the case then check if all the connections are done properly. Also check if there are any issues like wrong naming, capitalization issues or parameter value issues.
  - If the signals are right but the mimicking is not happening then verify the following:
    - For what range of frequencies is the VCO working properly: Is our required output frequency range lying in the VCO's working range.
    - Is the Phase Frequency Detector able to detect the differences: If the phase difference is very small, the PFD might not be able to detect it.
    - Is the rate of Charge Pump output charging and discharging fast: Is it too fast or too slow? Is there too much fluctuations in charging or discharging? This means that the transistor sizing is the thing to pay attention to. Check the response of the CP when 0V is given as the input. If it is still charging then the charge leakage is the issue.
    - Whether the loop filter values are working out: This can be found out using the thumb rules


# Part 12: Layout design
 - To open magic first enter the directory containing the "sky130A.tech" file using the "cd" commands.
 - Type magic -T sky130A.tech in the terminal window.
 - The way to draw here in magic is first to make a box and then fill it with a material.
 - By left clicking, we fix the left bottom corner of the box.
 - By right clicking, we fix the right top corner of the box.
 - If we hover upon a layer (layers panel is present on the right hand side of the window), we can see the name of the layer on the top right corner of the screen.
 - By middle clicking on a layer, the box gets filled by the selected layer.
 - Materials needed for the transistors:
   - P-diffusion for the PMOS and N-diffusion for the NMOS.
   - For the gate, polysilicon layer is needed.
 - DRC error means that there is a size issue or a closeness issue. To know what exactly the issue is, select some part of the error region and press the "?" button on the keyboard and then the error will show up on the "magic command window".
 - Before creating a PMOS, we must create an N-Well region and then place the PMOS over it.
 - To copy a transistor, make a box around the transistor and press "A" button on the keyboard. Now, place the cursor where we wish to copy it and press "C" button. If we press the "M" button then it becomes the move operation.
 - For placing Vdd and ground, place the metal1 layer.
 - To connect two transistors, use the loacal interconnect layer (locali) like a wire.
 - To connect two layers, look for contact layers in the tray. They are represented with a cross symbol.
 - As long as we do not connect a contact, two different layers will not touch even if they overlap.
 - If we get a DRC error at this point it means that either the size of the contact is too small or the region of metal1 and local interconnect around the contact is not enough.
 - To create a label, draw a line around the edge of the layer that we want to label. A line is drawn by making a box of zero thickness. Type the command label name in the magic command window. Name can be any name to be used as label.
 - To make a port, draw a box around the label and type port make in the magic command window.
 - We need to make ports because when we extract the parasitics from our design, the input and output ports will automatically have these names as we have labelled them as ports. 


# Part 13: Layout Walkthrough

  - We saw the previously designed magic files of the following circuits:
    - Frequency Divider
     <img width="1343" alt="FD post layout" src="https://user-images.githubusercontent.com/77117825/156919353-904cd3de-b47a-41f3-97eb-de11f4133ea8.png">
    - Phase Frequency Detector
      - In the PFD design, on the right side, there are two similar drawings on the top and on the bottom.
      - They are additional buffers that were kept for getting full swing.
     <img width="1239" alt="PFD post layout" src="https://user-images.githubusercontent.com/77117825/156919415-775476f1-29e8-4a5e-9544-c80f7c2167a9.png">
 
    - Charge Pump
      - Top most long transistor is just for enabling or disabling the charge pump.
      - Right below it is the upper current source. This much difference in the size of the transistor for the current source is to allow maximum current for the charging and discharging process.
      - Two inverters on the left create the UPz and the DOWNz signals (UPz = up bar and DOWNz = down bar or their inverted variants).
      <img width="1167" alt="CP post layout" src="https://user-images.githubusercontent.com/77117825/156919437-2748cb42-f7ef-4a5d-a581-bf756eae98d9.png">

    - Voltage Controlled Oscillator
      - There are seven inverters to reduce the range of frequencies that we are dealing with.
      - Only the last inverter in the inverter is large to improve the driving strength of the oscillator.
      - There is an additional inverter at the end and its purpose is to obtain a full swing for the output.
      - At the top there is a small PMOS that acts as an enable or disable for the VCO.
# Part 14: Parasitic Extraction 
 - In order to do the parasitics extraction follow the steps given below:

 - Open the magic file of the circuit that is to be extracted.
 - Press "I" button on the keyboard. It selects the whole design.
 - In the magic command window type extract all. This extracts the layout connectivity information into a ".ext" file.
 - Generally, if there are any warnings at this point, they would be because of wrong connections or short circuits or etc. But these are warnings and need not be errors.
 - Now, we need to convert the ".ext" file to a ".spice" file to use for simulations.
 - In the magic command window type ext2spice cthresh 0 rthresh 0 and then type ext2spice.
 - Here the cthreshold 0 and rthreshold 0 setting is given to tell magic that if any amount of capacitive or resistive effect is present, then we want to extract it.
 - In the spice we find a .option scale=10000u. Every parameter in the spice file is multiplies by this scale.
 - For our case, since we know that the scaling factor is 10n so change the .option command from scale=10000u to scale=10n.
<p align="center"><img src="https://user-images.githubusercontent.com/77117825/156920442-c7df11ee-b2c6-4993-8976-9989bd5f01af.jpeg"></p>

# Part 15: Post Layout simulations
 
 - First extract the spice file from the "PFD.mag" file (as done in [Part 14](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/blob/main/README.md#part-14-parasitic-extraction))
 - Now using the terminal enter the directory where "sky130nm.lib" and "PFD.spice" are saved using the "cd" command.
 - Type the command nano PFD_postlay.cir in the terminal 

       - .include spice_lib/sky130.lib
         .include PFD.spice

         xx1 Ref_Clk Up Down Clk2 GND VCC PFD

         v1 VDD GND 1.8v

         v2 Ref_Clk GND PULSE 0 1.8v 0 6p 6p 40n 80n

         v3 Clk2 GND PULSE 0 1.8v 10n 6p 6p 40n 80n

         .control 
         tran 0.1n 5u
         plot v(Up) v(Down)+2 v(Ref_Clk)+4 v(Clk2)+6
         .endc

         .end
    - In the terminal, it looks like:


<p align="center"><img width="831" alt="Screenshot 2022-03-06 at 4 47 17 PM" src="https://user-images.githubusercontent.com/77117825/156920731-cd902884-23b5-4170-a38c-2f3a7e1a17b6.png"></p>

   - Now save this file using the ctrl+S command and exit it using the ctrl+X command.
   - Run this file using ngspice as shown:
  
<p align="center"><img width="835" alt="Screenshot 2022-03-06 at 4 48 26 PM" src="https://user-images.githubusercontent.com/77117825/156920768-4973448c-07dd-4263-a8bf-20e7f4c6ee50.png"></p>

   - The zoomed version of the output looks like:
 
<p align="center"><img width="831" alt="Screenshot 2022-03-06 at 4 49 32 PM" src="https://user-images.githubusercontent.com/77117825/156920804-6d6f99b7-6d92-4cb6-aaf4-f8ec5a9ef1fe.png"></p>
 
   - Now we will simulate it for a phase difference between Ref_Clk and Clk2 to be 1ns.
   - To do so change the line from v3 Clk2 GND PULSE 0 1.8v 10n 6p 6p 40n 80n to v3 Clk2 GND PULSE 0 1.8v 1n 6p 6p 40n 80n
   - After doing this the file, opened in terminal should look like:

<p align="center"><img width="832" alt="Screenshot 2022-03-06 at 4 50 45 PM" src="https://user-images.githubusercontent.com/77117825/156920852-dcf9b351-05c3-4003-a7c9-374522c11cf4.png"></p>

   - The output now looks like:

<p align="center"><img width="835" alt="Screenshot 2022-03-06 at 4 51 42 PM" src="https://user-images.githubusercontent.com/77117825/156920877-0360b1b3-3254-4954-accc-5c09bb0593f1.png"></p>

   - The zoomed output looks like:

<p align = "center"><img width="835" alt="Screenshot 2022-03-06 at 4 52 31 PM" src="https://user-images.githubusercontent.com/77117825/156920907-c4d2dfe0-5a67-410d-9a8a-0d7f6501b1f5.png"></p>

   - So, the phase difference of even 1ns is detected.
   - This performance is possible because of the circuit that we have chosen for the PFD.
   - The default circuit that was discussed in the theory has the dead zone issue which doesn't allow the detection of such narrow phase differences.

# The post layout simulation
  - To simulate the circuit after layout we need to write the following code in the file "PLL_PostLay.cir":
     [Post Layout circuit.txt](https://github.com/tanmaycv-09/sky130PLLdesignWorkshop/files/8192434/Post.Layout.circuit.txt)
           
   - To simulate the code type first, enter the directory where the file is saved using the "cd" command in terminal.
   - Now simulate the file using the ngspice command 
   - We are going to receive two plot outputs for this file because of the following lines in the ".control" instruction:
     
         -.control
         tran 0.1ns 180us
         plot v(Clk_Ref) v(Clk_Out_by_8) v(Clk_Out_by_4)+2 v(Clk_Out_by_2)+4 v(Clk_Out)+6 v(up)+8 v(down)+10 v(VCtrl)+12
         plot v(Clk_Ref) v(Clk_Out_by_8)+2
         .endc 
     
     - The first post-layout plot that we receive is much like the pre-layout plot:
   <p align="center"><img width="833" alt="Screenshot 2022-03-06 at 5 04 27 PM" src="https://user-images.githubusercontent.com/77117825/156921237-16ed43fb-42dd-4388-8cd8-70936f9398fc.png"></p>
   
   - When we zoom into into it we get:
 
   <p align="center"> <img width="835" alt="Screenshot 2022-03-06 at 5 05 37 PM" src="https://user-images.githubusercontent.com/77117825/156921292-eb871870-8d14-4e4c-b181-4e23049ccc63.png"></p>

 - We zoom in further into the plot to get:

  <p align="center"><img width="835" alt="Screenshot 2022-03-06 at 5 05 37 PM" src="https://user-images.githubusercontent.com/77117825/156921315-3e3f4805-4a6c-4c1d-b78f-8ed3b685d6dc.png"></p>
  
   - Finally when we zoom in onto the region 99.9us and 100us we get:


<p align="center"><img width="836" alt="Screenshot 2022-03-06 at 5 07 10 PM" src="https://user-images.githubusercontent.com/77117825/156921336-b475337e-1e01-4a86-9abb-629a1561004c.png"></p>

 - The second plot that we get is the output clock and reference clock signals plotted as:
<p align="center"><img width="834" alt="Screenshot 2022-03-06 at 5 08 05 PM" src="https://user-images.githubusercontent.com/77117825/156921376-9b61c58c-91fa-44d7-aa76-73ae1a30bee7.png"></p>

 - When we zoom into it we get:

<p align="center"><img width="834" alt="Screenshot 2022-03-06 at 5 09 08 PM" src="https://user-images.githubusercontent.com/77117825/156921405-69a5b918-373d-462d-b5d0-2b853f62708b.png"></p>

   - For the mcq on post layout simulation
     
      - Create a new directory containing "sky130nm.lib" and "PFD.spice" in it.
      - Enter this directory through the terminal and type the command nano PFD_postlay.cir
      - Type the following code in it:
      
            - .include sky130nm.lib
              .include PFD.spice

              xx1 Ref_Clk Up Down Clk2 GND VCC PFD

              v1 VDD GND 1.8v
              
              v2 Ref_Clk GND PULSE 0 1.8v 0 6p 6p 40n 80n

              v3 Clk2 GND PULSE 0 1.8v 0.25n 6p 6p 40n 80n
              
              .control 
              tran 0.1n 5u
              plot v(Up) v(Down)+2 v(Ref_Clk)+4 v(Clk2)+6
              .endc

              .end    
              
              
       - In the terminal, it looks like:
<p align="center"><img width="835" alt="Screenshot 2022-03-06 at 5 12 39 PM" src="https://user-images.githubusercontent.com/77117825/156921500-4dfb1287-905d-46e2-8168-f25137af09e9.png"></p>

  - The output that we receive looks like:

<p align="center"><img width="841" alt="Screenshot 2022-03-06 at 5 13 33 PM" src="https://user-images.githubusercontent.com/77117825/156921529-10781cc8-3f32-48a0-be55-5d7b06a54418.png"></p>

 - And the zoomed output looks like:
<p align="center"><img width="838" alt="Screenshot 2022-03-06 at 5 14 13 PM" src="https://user-images.githubusercontent.com/77117825/156921550-be5df890-c67e-4ae1-9bb1-57d27a35e1ff.png"></p>

  - In this case the up signal is just a pulse because of the "Dead zone".

# The final PLL layout looks as follows:

<p align="center"><img width="834" alt="Screenshot 2022-03-06 at 5 15 40 PM" src="https://user-images.githubusercontent.com/77117825/156921605-b5bdf4ba-193a-4793-9a99-574d9e0e3ad8.png"></p>


# Conclusion, Opinion & Challenges 
This workshop was very helpful as it increased my knowledge on the subject PLL, i also learnt about how to use the tools and did all the simulations. This workshop cleared all my basic concepts of the PLL and the experience of getting a Silicon Proven Model ready was amazing. I stucked during the 2nd day at a point when i was not able to run the simulation because i was not including the files through the right path and i almost spend my half day in solving the error, i was so disappointed to a point that i thought of giving up, but Kunal Sir and Lakshmi mam they keep on answering my questions in the slack channel and around 6 p.m. i found out what was the problem.
After that i completed the whole day work in just 5 hrs. This was one of the greatest challenges that i faced in my life.

# References

https://github.com/lakshmi-sathi/avsdpll_1v8

https://github.com/VrushabhDamle/sky130PLLdesignWorkshop






 
