# Phase-Locked Loop(PLL) IC design on Open-Source Google-Skywater 130nm Workshop

<p align="center"><a href="https://www.vlsisystemdesign.com/pll-design-using-sky130/"><img width="833" alt="Screenshot 2022-03-06 at 9 00 55 AM" src="https://user-images.githubusercontent.com/77117825/156908010-9d44ba40-339d-4047-97e7-cbef360f11a5.png"></a></p>

# Brief Description of the Workshop
  
 This workshop was of 2 days conducted by VLSI System Design on the topic Phase-Locked Loop(PLL) IC design on Open-Source Google-Skywater 130nm. Over the course of two days this workshop explains the PLL inside out and we also have to design a PLL using certain simulation softwares (ngspice and magic). On the first day of the workshop, we are introduced to PLL and we discuss the circuitry required to design one. We discuss in depth about the individual components used and their role in operating the PLL. We also install the softwares on the first day and this concludes the first day. On the second day of the workshop, we start by preparing spice files and conducting spice simulations of individual components of the PLL. Then using these components, we create a spice file that will work effectively as a complete PLL. After this, we look into the layouts of the individual components and finally combine them to form a final PLL layout. We perform post-layout simulations on the individual components and end the day with discussion on tapeouts and Caravel.

  # Table of Contents 
  
  
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

# Part 11: Steps to combine PLL sub-circuits and PLL full design simulation

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





                      
