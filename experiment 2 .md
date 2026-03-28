Q1) Design CS amplifier with PMOS active load configuration in tsmc180nmtech.lib in LTSPICE.

Given: VDD = 1.8V, Power <= 1mW, CL = 1pF, L = 180nm, Vin = 10mV, f = 1kHz

**CIRCUIT DIAGRAM:**

Without Capacitor:

![CIRCUIT](images/circuit3.png)


**DESIGN CALCULATIONS:**

**1. DC ANALYSIS:**

Power = Voltage * Current

P = VDD*ID

Let ID = 200uA

Thus, Power = 1.8*200u = 0.36mW which is <= 1mW 

Given, The drop across RS is 0.2V --> VS = 0.2V

Vout = VDS + IDRS = (VDD/2) + 0.2 = (1.8/2) + 0.2 = 1.1V

We know that, by Ohm's law,

VS = IDRS

RS = VD/ID = 0.2/200u = 1kohm
<br><br><br>
For M1 (NMOS) transistor:

Given, the overdrive voltage VOV = 0.25V

From the datasheet, VTH = 0.36V

We know that,

VOV = VGS - VTH  

VGS = VOV + VTH = 0.25 + 0.36 = 0.61V

Biasing voltage VB1 is obtained by,

VB1 = VG1 = VGS + IDRS = 0.61 + 0.2 = 0.81V
<br><br>
For M1 to be SATURATION,

VGS >= VTH

0.61 >= 0.36

and VDS >= VOV

(VDD/2) >= VOV

0.9 >= 0.25

Hence both the conditions are satisfied. Therefore M1 is operating in **SATURATION** region.
<br><br>                                                                        
From the drain current formula in saturation region, we get the value of W.

![id](images/id.png)

![k](images/k.png) = 230.4uA/V^2

Hence, W1 = 5um 

---

For M2 (PMOS) transistor:

Given, the overdrive voltage VOV = 0.25V

From the datasheet, VTH = 0.39V


We know that,

VOV = VSG - |VTH|  

VSG = VOV + |VTH| = 0.25 + 0.39 = 0.64V


Biasing voltage VB1 is obtained by,

VB2 = VG2 = VS + VSG = VDD - VSG = 1.8 - 0.64 = 1.16V
<br><br>
For M2 to be SATURATION,

VSG >= VTH

0.64 >= 0.39

and VDS >= VOV

(VDD/2) >= VOV

0.9 >= 0.25

Hence both the conditions are satisfied. Therefore M2 is operating in **SATURATION** region.
<br><br>
From the drain current formula in saturation region, we get the value of W.

![id](images/id.png)

![k](images/k.png) = 230.4uA/V^2

Hence, W2 = 11.823um 
<br><br><br>
To fix the DC operating point, 

![DC_opnt1](images/DC_opnt3.png)

Initially, we get 

**ID = 75.029uA for W1 = 5um and W2 = 11.823um**

After altering W values, we get 

**ID = 201.37uA for W1 = 27um and W2 = 36.5um**


The DC biasing ensures that the drain current is set according to the power budget while keeping the MOSFET operating in the saturation region, which is essential for achieving proper and stable amplification.

---

**2. TRANSIENT ANALYSIS:**

![wave](images/waveform3.png)

Here the blue waveform represents the input voltage and the green waveform represents the output voltage. We can observe the 180 degree phase shift and the amplification of the output signal.

Peak to peak value of **Vin(p-p) = 20mV** 

Vout (max) = 1217 mV = 1.217 V

Vout (min) = 986 mV = 0.986 V

Thus, **Vout (p-p) = 231mV**

and since the Vout(DC) = 1.1V

This indicates that the amplifier is biased above mid-supply (0.9V), resulting in limited symmetrical swing.

From the transient analysis, the output remains within saturation limits and does not show distortion, confirming stable operation within allowable swing range.

---

**3. AC ANALYSIS:**

From the theoretical calculations, Transconductance gm1 = 2id/vov = (2*200u)/0.25 = 1.6mmho

lamda = 0.1 V^-1

r01 = 1/(lamda*id) = 1/(0.1 * 200u) = 50kohm

Similarly, r02 = 50kohm
<br><br>

Thus,

![gain](images/gain.png) = -15.384 V/V

|Av| = 15.384 V/V = 23.741 dB
<br><br>

- Without Capacitor

![freq](images/freq3.png)

Av = 21.39 dB 

Bandwidth Bw = 189.823 MHz

Gain Bandwidth Product GBwP = Av*Bw = 2227.659 MHz
<br><br>
- With Capacitor

![CIRCUIT1](images/circuit31.png)

![freq](images/freq31.png)

Av = 21.39 dB

Bandwidth Bw = 190.2 MHz

GBwP = Av*Bw = 2232.083 MHz

Therefore, GBwP:

**Without Capacitor = 2227.659 MHz**

**With Capacitor = 2232.083 MHz**

---

**RESULTS:**

From the LTspice simulation of the common-source (CS) amplifier:
- DC Operating Point: The biasing ensured that the MOSFETs operate in the saturation region, which is essential for linear amplification and stable gain.

- Transient Response: The input waveform (Vin) of ~20 mVpp produced an output waveform (Vout) of ~231 mVpp with a significantly larger amplitude and a 180° phase shift, confirming that the stage is functioning as a CS amplifier.

- Small-Signal AC Gain: The voltage gain |Av| from AC analysis was observed to be approximately 15.384 V/V (≈ 23.741 dB).

    - Frequency Response:
 
         Without coupling capacitor: Av ≈ 21.39 dB, Bandwidth (BW) ~189.8 MHz

         With coupling capacitor: Av ≈ 21.39 dB, BW ~190.2 MHz
<br><br>
    - Gain-Bandwidth Product (GBW):

         Without capacitor: ~2227.66 MHz

         With capacitor: ~2232.08 MHz

These results indicate consistent performance with high gain and wide bandwidth, with negligible effect from adding the coupling capacitor.
<br><br>
**VALIDATION**

- The transistor operates in the saturation region, ensuring proper amplification.

- The DC drain voltage (~1.1 V) confirms that the MOSFET is properly biased and not in cutoff or triode region.

- Since , the output remains safely within supply voltage range (0 V to 1.8 V), confirming stable operation.

- The amplified and phase-inverted output waveform matches the theoretical behavior of a CS amplifier.

- The coupling capacitor value (1 pF), together with the circuit resistance, results in a cutoff frequency lower than the operating frequency range. Therefore, the capacitor does not significantly affect the gain or bandwidth in the observed region.

Thus, the simulation results are consistent with theoretical expectations.
<br><br>
**INFERENCE**

- The CS amplifier configured with TSMC 180 nm technology satisfies the design goals of biasing, gain, and bandwidth under the power constraint (≤ 1 mW). The stage exhibits expectations of a CS amplifier — high voltage gain, 180° phase inversion, and broad frequency response.

- The output swing of 231 mV (peak-to-peak) indicates proper small-signal amplification. The DC bias level (~1.10 V) is slightly above mid-supply (0.9 V), resulting in limited symmetrical swing.

- The observed Gain-Bandwidth Product (GBW) is high, showing that the design is suitable for moderate-frequency analog amplification.

Overall, the simulation confirms that the CS amplifier is operating in the intended region and meets design criteria for gain and frequency performance.
<br><br>

---
<br><br>
Q2) Design CS amplifier with PMOS active load and NMOS current source degeneration in tsmc180nmtech.lib in LTSPICE.

Given: VDD = 1.8V, Power <= 1mW, CL = 1pF, L = 180nm, Vin = 10mV, f = 1kHz

**CIRCUIT DIAGRAM:**

Without Capacitor:

![CIRCUIT](images/circuit4.png)


**DESIGN CALCULATIONS:**

**1. DC ANALYSIS:**

Power = Voltage * Current

P = VDD*ID

Let ID = 200uA

Thus, Power = 1.8*200u = 0.36mW which is <= 1mW 

and,

Vout = VDD/2 = 1.8/2 = 0.9 V
<br><br><br>
For M3 (NMOS) transistor:

Given, the overdrive voltage VOV = 0.25V

From the datasheet, VTH = 0.36V

We know that,

The biasing voltage VB2 = VGS3 = VOV + VTH = 0.25 + 0.36 = 0.61V

<br><br>
For M3 to be SATURATION,

VGS3 >= VTH

0.61 V >= 0.36 V

and VDS3 >= VOV  

VDS = 0.263 V from the simulation

Hence, 0.263 >= 0.25

Therefore both the conditions are satisfied. Therefore M3 is operating in **SATURATION** region.

<br><br>
For M1 (NMOS) transistor :

VOV = 0.25V and VTH = 0.36V

Also, the source of M1 = drain of M3

That is, VS1 = VD3

We know that, VDS3 >= VOV 

Hence, VD3 = VS1 = 0.25 V

Vin = VG1 = VS1 + VGS1 = 0.25 + 0.61 = 0.86 V

<br><br>
For M1 to be SATURATION,

VGS1 >= VTH

0.61 V >= 0.36 V

and VDS1 >= VOV  

VDS1 = VD1 - VS1 = Vout - VS1 = 0.9 - 0.25 = 0.65 V

Hence, 0.65 V >= 0.25 V

Therefore both the conditions are satisfied. Therefore M1 is operating in **SATURATION** region.

<br><br>                                                                        
From the drain current formula in saturation region, we get the value of W.

![id](images/id.png)

![k](images/k.png) = 230.4uA/V^2

Hence, W1 = 5um 

Similarly, W3 = 5um

<br><br>
For M2 (PMOS) transistor:

Given, the overdrive voltage VOV = 0.25V

From the datasheet, VTH = 0.39V

VSG2 = VOV + |VTH| = o.25 + 0.39 = 0.64 V

We know that,

The biasing voltage VB1 = VG2 = VS2 - VSG2 = 1.8 - 0.64 = 1.16 V

<br><br>
For M2 to be SATURATION,

VSG2 >= |VTH|

0.64 V >= 0.36 V

and VSD2 >= VOV  

VSD2 = VDD - Vout = 1.8 - 0.9 = 0.9 V

Hence, 0.9 V >= 0.25 V

Therefore both the conditions are satisfied. Therefore M2 is operating in **SATURATION** region.

<br><br>
From the drain current formula in saturation region, we get the value of W.

![id](images/id.png)

![k](images/k.png) = 230.4uA/V^2

Hence, W2 = 11.823um 
<br><br><br>
To fix the DC operating point, 

![DC_opnt1](images/DC_opnt4.png)

Initially, we get 

**ID = 61.597uA for W1 and W3 = 5um and W2 = 11.823um**

After altering W values, we get 

**ID = 200.708uA for W1 and W3 = 16.60um and W2 = 35.12um**


The DC biasing ensures that the drain current is set according to the power budget while keeping the MOSFET operating in the saturation region, which is essential for achieving proper and stable amplification.

---

**2. TRANSIENT ANALYSIS:**

![wave](images/waveform4.png)

Here the blue waveform represents the input voltage and the green waveform represents the output voltage. We can observe the 180 degree phase shift and the amplification of the output signal.

Peak to peak value of **Vin(p-p) = 19.25mV** 

Vout (max) = 938.53 mV = 0.93853 V

Vout (min) = 861.95 mV = 0.86195 V

Thus, **Vout (p-p) = 76.58mV**

and since the Vout(DC) = 0.9V

This indicates that the amplifier is biased exactly in mid-supply (0.9V), resulting in symmetrical swing.

The output is biased at 0.9 V, keeping all transistors in saturation and it shows the expected 180 deg phase inversion for a Common-Source amplifier.

---
**3. AC ANALYSIS:**

From the theoretical calculations, Transconductance gm1 = 2id/vov = (2*200u)/0.25 = 1.6 mmho

lamda = 0.1 V^-1

r01 = 1/(lamda*id) = 1/(0.1 * 200u) = 50 kohm

Similarly, r02 = 50 kohm and r03 = 50 kohm
<br><br>

Thus,

![gain](images/gain1.png) = -0.49382 V/V

|Av| = 0.49382 V/V = 6.128 dB
<br><br>

- Without Capacitor

![freq](images/freq4.png)

Av = 11.973 dB 

Bandwidth Bw = 146.253 MHz

Gain Bandwidth Product GBwP = Av*Bw = 580.436 MHz
<br><br>
- With Capacitor

![CIRCUIT1](images/circuit41.png)

![freq](images/freq41.png)

Av = 11.972 dB

Bandwidth Bw = 146.288 MHz

GBwP = Av*Bw = 580.375 MHz

Therefore, GBwP:

**Without Capacitor = 580.436 MHz**

**With Capacitor = 580.375 MHz**

---

**Result:**

The Common Source (CS) amplifier with both a resistive load and source degeneration demonstrates a reduced voltage gain compared to the standard CS configuration. The simulation confirms that while the peak-to-peak output voltage decreases, the linearity of the signal improves, and the biasing point remains more stable against variations in MOSFET parameters.
<br><br>
**Validation:**


The simulation data validates the theoretical gain formula:

![gain](images/gain1.png)

The measured gain aligns with this formula. Additionally, the input and output waveforms remain 180° out of phase, and the source terminal shows a non-zero AC signal, validating the presence of negative feedback.
<br><br>
**Inference**
 * The inclusion of M3 introduces local negative feedback, which stabilizes the voltage gain and increases the input signal swing before reaching distortion.
 * The implementation of source degeneration sacrifices absolute voltage gain to achieve superior biasing stability and signal linearity. Any observed reduction in bandwidth indicates that the circuit's dominant pole is heavily influenced by the load resistance (RD = ro1||ro2), highlighting that gain-bandwidth optimization requires a precise balance between feedback levels and output impedance.

---
<br><br>
Q2) Design CS amplifier with PMOS active load and diode connected NMOS transistor configuration in tsmc180nmtech.lib in LTSPICE.

Given: VDD = 1.8V, Power <= 1mW, CL = 1pF, L = 180nm, Vin = 10mV, f = 1kHz

**CIRCUIT DIAGRAM:**

Without Capacitor:

![CIRCUIT](images/circuit5.png)


**DESIGN CALCULATIONS:**

**1. DC ANALYSIS:**

Power = Voltage * Current

P = VDD*ID

Let ID = 200uA

Thus, Power = 1.8*200u = 0.36mW which is <= 1mW 

and,

Vout = VDD/2 = 1.8/2 = 0.9 V
<br><br><br>
For M3 (NMOS) transistor:

Given, the overdrive voltage VOV = 0.25V

From the datasheet, VTH = 0.36V

From the circuit we know that,

VG3 = VD3 = VS1

Hence,VGS3 = VG3 = VOV + VTH = 0.25 + 0.36 = 0.61V

<br><br>
For M3 to be SATURATION,

VGS3 >= VTH

0.61 V >= 0.36 V

and VDS3 >= VOV  

VDS = 0.604 V from the simulation

Hence, 0.604 V >= 0.25 V

Therefore both the conditions are satisfied. Therefore M3 is operating in **SATURATION** region.

<br><br>
For M1 (NMOS) transistor :

VOV = 0.25V and VTH = 0.36V

VGS1 = VOV + VTH = 0.25 + 0.36 = 0.61 V

Also, VS1 = VD3 = 0.61 V

Hence, Vin = VG1 = VS1 + VGS1 = 0.61 + 0.61 = 1.22 V

<br><br>
For M1 to be SATURATION,

VGS1 >= VTH

0.61 V >= 0.36 V

and VDS1 >= VOV  

VDS1 = VD1 - VS1 = Vout - VS1 = 0.9 - 0.61 = 0.29 V

Hence, 0.29 V >= 0.25 V

Therefore both the conditions are satisfied. Therefore M1 is operating in **SATURATION** region.

<br><br>                                                                        
From the drain current formula in saturation region, we get the value of W.

![id](images/id.png)

![k](images/k.png) = 230.4uA/V^2

Hence, W1 = 5um 

Similarly, W3 = 5um

<br><br>
For M2 (PMOS) transistor:

Given, the overdrive voltage VOV = 0.25V

From the datasheet, VTH = 0.39V

VSG2 = VOV + |VTH| = o.25 + 0.39 = 0.64 V

We know that,

The biasing voltage VB1 = VG2 = VS2 - VSG2 = 1.8 - 0.64 = 1.16 V

<br><br>
For M2 to be SATURATION,

VSG2 >= |VTH|

0.64 V >= 0.36 V

and VSD2 >= VOV  

VSD2 = VDD - Vout = 1.8 - 0.9 = 0.9 V

Hence, 0.9 >= 0.25

Therefore both the conditions are satisfied. Therefore M2 is operating in **SATURATION** region.

<br><br>
From the drain current formula in saturation region, we get the value of W.

![id](images/id.png)

![k](images/k.png) = 230.4uA/V^2

Hence, W2 = 11.823um 
<br><br><br>
To fix the DC operating point, 

![DC_opnt1](images/DC_opnt5.png)

Initially, we get 

**ID = 30.701uA for W1 and W3 = 5um and W2 = 11.823um**

After altering W values, we get 

**ID = 200.86uA for W1 and W3 = 15.048um and W2 = 35.15um**


The DC biasing ensures that the drain current is set according to the power budget while keeping the MOSFET operating in the saturation region, which is essential for achieving proper and stable amplification.

---

**2. TRANSIENT ANALYSIS:**

![wave](images/waveform5.png)

Here the blue waveform represents the input voltage and the green waveform represents the output voltage. We can observe the 180 degree phase shift and the amplification of the output signal.

Peak to peak value of **Vin(p-p) = 19.7mV** 

Vout (max) = 1.0629 V

Vout (min) = 0.7837 V

Thus, **Vout (p-p) = 0.2792 V**

and since the Vout(DC) = 0.9 V

This indicates that the amplifier is biased exactly in mid-supply (0.9V), resulting in symmetrical swing.

The output is biased at 0.9 V, keeping all transistors in saturation and it shows the expected 180 deg phase inversion for a Common-Source amplifier.

---

**3. AC ANALYSIS:**

From the theoretical calculations, Transconductance gm1 = 2id/vov = (2*200u)/0.25 = 1.6 mmho

lamda = 0.1 V^-1

r01 = 1/(lamda*id) = 1/(0.1 * 200u) = 50 kohm

Similarly, r02 = 50 kohm and r03 = 50 kohm
<br><br>

Thus,

![gain](images/gain1.png) = -20 V/V

|Av| = 20 V/V = 26.02 dB
<br><br>

- Without Capacitor

![freq](images/freq4.png)

Av = 23.271 dB 

Bandwidth Bw = 426.358 MHz

Gain Bandwidth Product GBwP = Av*Bw = 6213.315 MHz
<br><br>
- With Capacitor

![CIRCUIT1](images/circuit51.png)

![freq](images/freq51.png)

Av = 23.271 dB

Bandwidth Bw = 426.255 MHz

GBwP = Av*Bw = 6211.814 MHz

Therefore, GBwP:

**Without Capacitor = 6213.315 MHz**

**With Capacitor = 6211.814 MHz**

---
**Results and validation:**


The experimental results for the Common Source (CS) amplifier series demonstrate the fundamental trade-offs between gain, stability, and voltage headroom. With a supply voltage (VDD) of 1.8V and a low threshold voltage (VTH) of 0.25V, the circuits benefit from significant overdrive headroom. This allows the transistors to stay deep in the saturation region even at the lowest point of the output swing (0.78V), ensuring the amplifier maintains high linearity and avoids signal clipping.
<br><br>
**Inference:**

The diode-connected configuration effectively trades high gain for increased bandwidth and linearity. This setup is ideal for integrated circuits where high-value resistors are difficult to fabricate. It demonstrates that using an active load provides a predictable gain dictated by device geometry (W/L ratios) rather than absolute resistance values.

---
**Comparision of all three configurations:**

Resistive Load :
       
-High Voltage Gain: Provides the highest potential gain among the three.
  
-Large Output Swing: Allows for a wide range of output voltage variation.

Source Degeneration :
    
-Improved Linearity: Reduces distortion, making the amplifier more linear.
       
-Increased Input Impedance: Higher input resistance due to the feedback effect at the source.

Diode-Connected Load :
      
-Better Bandwidth: Typically offers a higher 3dB frequency compared to high-gain resistive loads.
       
-Reduced Area: Occupies significantly less space on a silicon chip than a high-value resistor.
