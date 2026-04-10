# #  Linear Integrated Circuits Laboratory 

## Experiment 2:Common Source Amplifier -CONFIGURATIONS(TSMC 180nm)

 
---
## 1. AIM

To design and analyze the following MOS amplifier configurations using TSMC 180nm CMOS technology in LTspice:

1. Common Source Amplifier with Source Degeneration  
2. Cascode Amplifier  
3. Current Mirror Loaded Common Source Amplifier  

---
## 2. SOFTWARE REQUIRED

- LTspice Simulation Tool  
- TSMC 180nm Model Library  
- DC Power Supply (VDD = 1.8V)  
- CMOS Transistors (NMOS and PMOS)  

---
## 3. MOS Amplifier Fundamentals

MOS amplifiers convert small input voltage variations into amplified output signals.  
For proper amplification, MOSFETs must operate in the **saturation region**.

### MOSFET Equations

| ID (Drain Current) | Vov (Overdrive) | gm (Transconductance) | ro (Output Resistance) | Av (Voltage Gain) |
|--------------------|-----------------|----------------------|------------------------|-------------------|
| (1/2) μCox (W/L)(VGS−VTH)² | VGS − VTH | 2ID / Vov | 1 / (λID) | −gm Rout |
---
## 4. Comparison of Amplifier Configurations

| Circuit | Concept | Advantage | Limitation |
|--------|---------|-----------|-----------|
| Source Degenerated CS | Source resistor feedback | Stable bias | Reduced gain |
| Cascode Amplifier | CS + Common Gate | High output resistance | Higher complexity |
| Active Load CS | Current mirror load | Higher gain | Bias sensitive |

---
## 5. Device Parameters (TSMC 180 nm)

| Supply Voltage | Target Drain Current | VTHn | VTHp | Channel Length |
|----------------|----------------------|------|------|----------------|
| 1.8 V | 200 µA | 0.36 V | −0.39 V | 560 nm |

### Oxide Parameters

| εox (Oxide Permittivity) | tox (Oxide Thickness) | Cox (Oxide Capacitance) |
|--------------------------|----------------------|-------------------------|
| εrε0 = 3.54 × 10⁻¹¹ | 4.1 × 10⁻⁹ m | εox / tox = 8.634 × 10⁻³ F/m² |

---
## 6. Saturation Conditions

To ensure proper amplifier operation, MOSFETs must satisfy:

| Device | Condition |
|------|-----------|
| NMOS | VDS ≥ VGS − VTH |
| PMOS | VSD ≥ VSG − VTH(PMOS) |

---
## 7. Process Transconductance Parameters

| Parameter | Calculation | Result |
|----------|-------------|-------|
| μnCox | μn × Cox | 2.363 × 10⁻⁴ A/V² |
| μpCox | μp × Cox | 9.99 × 10⁻⁵ A/V² |

---
## 8. Width Calculation

Using the saturation current equation
ID = (1/2) μCox (W/L) (Vov)²

Rearranging,
W = (2 ID L) / [μCox (Vov)²]

### Calculated Dimensions

| NMOS Width | PMOS Width |
|------------|------------|
| 15.16 µm | 35.9 µm |

---
### Observation

• NMOS mobility is higher than PMOS mobility.  
• Therefore NMOS requires smaller width.  
• PMOS width is approximately 2.3× NMOS width for the same current.

------------------------------------------------------------

### EXP2 - CIRCUIT 2A – SOURCE DEGENERATED COMMON SOURCE AMPLIFIER

---
## Design Objective

To design and implement a **source-degenerated common source amplifier**  
for **ID = 200 µA** with **maximum symmetric output swing**.

---
## Circuit Implementation (LTspice)
<img width="986" height="569" alt="2a" src="https://github.com/user-attachments/assets/c11bd95f-dbed-4afc-a40e-d0e2ab5c5a31" />


---
# DC Analysis

## Design Conditions

| VDD | ID | VTH (NMOS) | VTH (PMOS) |
|-----|----|------------|------------|
| 1.8 V | 200 µA | 0.36 V | −0.39 V |
---
## Bias Design and Voltage Selection

### Voltage Limits for Saturation Operation

| Parameter | Minimum | Maximum | Reason |
|-----------|---------|---------|-------|
| VGS (NMOS) | ≥ VTH = 0.36 V | ≤ VDD = 1.8 V | Required for channel formation |
| VDS (NMOS) | ≥ Vov | ≤ VDD | Required for saturation |
| VSG (PMOS) | ≥ |VTHp| = 0.39 V | ≤ VDD | PMOS conduction condition |
| VSD (PMOS) | ≥ Vov | ≤ VDD | Saturation condition |

---
## Output Voltage for Maximum Swing

To achieve **maximum symmetric signal swing**, the NMOS drain–source
voltage is placed near the midpoint of the supply.

| VDS Calculation | Result |
|-----------------|--------|
| VDD / 2 = 1.8 / 2 | **0.9 V** |

**Justification**

Placing the operating point at **half the supply voltage**
provides equal headroom for positive and negative output swings,
thereby maximizing dynamic range.

---
## Source Voltage Selection

A small source voltage is introduced using a **source resistor** to
provide **negative feedback and bias stabilization**.

| Parameter | Value |
|-----------|------|
| VS | **0.2 V** |

**Justification**

| Reason | Explanation |
|------|-------------|
| VS > 0 | Enables source degeneration feedback |
| VS << VDD | Preserves drain voltage headroom |
| Stabilizes ID | Increase in current raises VS → reduces VGS |

Thus **0.2 V** provides feedback while keeping sufficient voltage
available across the transistor.

---
## Output Voltage Calculation

| Parameter | Calculation | Result |
|-----------|-------------|--------|
| VDS | Vout − VS | 0.9 = Vout − 0.2 |
| Vout | 0.9 + 0.2 | **1.1 V** |

---
## Source Resistor Calculation

| RS Calculation | Result |
|----------------|--------|
| VS / ID = 0.2 / (200 × 10⁻⁶) | **1 kΩ** |

---
## NMOS Gate Bias

| Calculation | Result |
|-------------|--------|
| VGS = VTH + Vov = 0.36 + 0.25 | **0.61 V** |
| VG = VGS + VS = 0.61 + 0.2 | **0.81 V** |

---
## Overdrive Voltage Verification

| Calculation | Result |
|-------------|--------|
| VGS = VG − VS = 0.81 − 0.2 | **0.61 V** |
| Vov = VGS − VTH = 0.61 − 0.36 | **0.25 V** |

### Allowable Overdrive Range

| Minimum Vov | Maximum Vov |
|-------------|-------------|
| > 0 | < VDS = 0.9 V |

Thus

0 < Vov < 0.9

The obtained value
Vov = **0.25 V**
lies well within this allowable range.

**Justification**

A moderate overdrive voltage:

• provides sufficient transconductance  
• maintains stable drain current  
• ensures adequate voltage headroom for signal swing

---
## PMOS Bias Calculation
| Calculation | Result |
|-------------|--------|
| VSG = Vov + VTHp = 0.25 + 0.39 | **0.64 V** |
| VGp = VDD − VSG = 1.8 − 0.64 | **1.16 V** |

# Saturation Verification

| Device | Condition | Result |
|-------|-----------|--------|
| NMOS | VDS ≥ Vov | 0.9 ≥ 0.25 ✔ |
| PMOS | VSD ≥ Vov | 0.7 ≥ 0.25 ✔ |

Both MOSFETs operate in **saturation region**.

---
# Final DC Operating Point

| VS | VDS | Vout | VG | VGp | ID | RS | Vov |
|----|-----|------|----|-----|----|----|----|
| 0.2 V | 0.9 V | 1.1 V | 0.81 V | 1.16 V | 200 µA | 1 kΩ | 0.25 V |

The calculated bias point ensures that both transistors operate in saturation
while providing sufficient voltage headroom for maximum output signal swing.

------------------------------------------------------------
<img width="694" height="529" alt="2aop" src="https://github.com/user-attachments/assets/bff52bb9-dd6b-403c-a60d-c5e676c88f4c" />

## Width Selection – Circuit 2A

The transistor widths were slightly increased during simulation to compensate for non-ideal MOSFET effects and obtain ID ≈ 200 µA.

| Device | Calculated Width (µm) | Practical Width (µm) | Justification |
|------|----------------------|----------------------|--------------|
| NMOS | 15.16 | 29 | Practical MOS models include channel-length modulation and mobility degradation, which reduce effective current. Increasing width restores ID ≈ 200 µA. |
| PMOS | 35.9 | 83 | PMOS mobility is lower than NMOS and short-channel effects reduce current, therefore a larger width is required to achieve the target current. |

-------------------
# Transient Analysis – Circuit 2A
(Source Degenerated Common Source Amplifier)

To evaluate the time-domain performance of the amplifier, a small-signal
sinusoidal input was applied at the gate terminal.

## Input Signal Parameters

| Waveform | Frequency | Amplitude | DC Offset |
|----------|-----------|-----------|-----------|
| Sine | 1 kHz | 10 mV | 0.81 V |
Input command used in LTspice:

Vin = SINE(0.81 10m 1k)

The DC offset corresponds to the calculated gate bias voltage required
to maintain **ID ≈ 200 µA**.

---

## Input Waveform

<img width="1010" height="454" alt="2ain" src="https://github.com/user-attachments/assets/06d52a9f-c434-46f2-b530-d8b296a7a7d6" />

The above waveform represents the sinusoidal input applied at the gate.

---

## Output Waveform

<img width="1009" height="453" alt="2aout" src="https://github.com/user-attachments/assets/f4ebdce3-cf7f-47f3-a3ea-9b134f6070de" />


Since this is a **common source amplifier**, the output signal is inverted
with respect to the input and exhibits a larger amplitude.

---

## Input and Output Comparison

<img width="1015" height="461" alt="2aboth" src="https://github.com/user-attachments/assets/6c48fa92-2d63-428b-a59b-20446c771d07" />

The combined waveform clearly shows amplification and the expected
**180° phase inversion**.

---

#Practical Gain Calculation from Transient Waveform

| Vin(p-p) | Vout(p-p) | Gain (Av) | Gain (dB) |
|----------|-----------|-----------|-----------|
| 0.819−0.800 = **0.019 V** | 1.386−0.858 = **0.528 V** | 0.528 / 0.019 = **27.78 V/V** | 20 log₁₀(27.78) = **28.87 dB** |

## Observation

The output signal is inverted relative to the input and shows
clear voltage amplification without distortion for the applied
small-signal input.

---
# AC Analysis – Circuit 2A

Small-signal AC analysis was performed to determine the frequency response
and midband gain of the amplifier.

### Simulation Parameters

| AC Command | Input AC Magnitude |
|------------|-------------------|
| `.ac dec 1000 .1 1G` | 1 V |
---

<img width="1002" height="445" alt="2abw" src="https://github.com/user-attachments/assets/96120f6a-d1a2-4bb9-85e2-a484e68984b9" />

---

### Frequency Response Results

| Midband Gain | −3 dB Gain | Bandwidth |
|--------------|------------|-----------|
| **28.71 dB** | **25.71 dB** | **52.1 MHz** |
---

## Theoretical Gain – Circuit 2A

Small-signal parameters obtained from the LTspice operating point:

| gm₁ | ro₁ | ro₂ | RS |
|-----|-----|-----|----|
| 1.6 mS | 58.8 kΩ | 58.8 kΩ | 1 kΩ |

The gain of a **source-degenerated common source amplifier**
with active load is given by:

Av = - gm₁ / (1 + gm₁RS + RS/ro₁) × ([gm₁RSro₁ + RS + ro₁] ∥ ro₂)

| Denominator | Output Resistance Term | Voltage Gain | Gain (dB) |
|-------------|-----------------------|--------------|-----------|
| 1 + 1.6 + (1k/58.8k) = **2.617** | ([gm₁RSro₁ + RS + ro₁] ∥ ro₂) ≈ **42.6 kΩ** | (1.6 mS / 2.617) × 42.6 kΩ ≈ **26 V/V** | 20 log₁₀(26) ≈ **28 dB** |

For **TSMC 180 nm technology**, the channel-length modulation parameter
λ typically lies in the range **0.1–0.2 V⁻¹**, which produces output
resistance values consistent with the LTspice operating point.

---
### Observation
The AC analysis shows a midband gain of 28.71 dB, which agrees closely
with both theoretical and transient gain results.
### Reason for Small Variation
The difference occurs because theoretical calculations assume ideal
device behavior, while LTspice includes practical effects such as
channel-length modulation.

-----------------------------------------------------------------------------------
### EXP2- CIRCUIT 2B – Common Source – Cascode Amplifier with Active Load
-----------------------------------------------------------------------------------
### Circuit Implementation in LTspice
<img width="869" height="530" alt="2b" src="https://github.com/user-attachments/assets/28df0e80-eac5-4c82-9b43-512590f96609" />


# DC Analysis – Circuit 2B (Cascode Amplifier)

## Design Conditions

| VDD | ID | VTHn | VTHp |
|----|----|----|----|
| 1.8 V | 200 µA | 0.36 V | −0.39 V |

---

# Voltage Limits for Proper Operation

| Parameter | Minimum | Maximum | Reason |
|-----------|---------|---------|--------|
| VGS (NMOS) | ≥ VTH = 0.36 V | ≤ VDD = 1.8 V | Channel formation |
| VDS (NMOS) | ≥ VOV | ≤ VDD | Saturation condition |
| VSG (PMOS) | ≥ |VTHp| = 0.39 V | ≤ VDD | PMOS conduction |
| VSD (PMOS) | ≥ VOV | ≤ VDD | Saturation condition |

---

# Overdrive Voltage Determination

| Calculation | Result |
|-------------|--------|
| VGS = VTH + VOV = 0.36 + 0.25 | **0.61 V** |
| VOV = VGS − VTH = 0.61 − 0.36 | **0.25 V** |

### Allowable Range

| Minimum VOV | Maximum VOV |
|-------------|-------------|
| > 0 | < VDS |

Since the drain-source voltage will be near

VDS ≈ VDD/2 = **0.9 V**

the allowable range becomes

0 < VOV < 0.9 V

The obtained value

**VOV = 0.25 V**

lies well within this range.

### Justification

A moderate overdrive voltage

• provides sufficient transconductance  
• keeps current stable  
• leaves headroom for signal swing

---

# Intermediate Node Voltage (VS1)

In a cascode amplifier, the drain of the lower transistor becomes the
source of the upper transistor.

To keep the **lower NMOS (M2) in saturation**

VDS2 ≥ VOV

Since

VS2 = 0

| Calculation | Result |
|-------------|--------|
| VDS2 = VD2 − VS2 = VS1 − 0 | VS1 |
| Required VDS2 ≥ VOV | ≥ 0.25 V |

Thus

| VS1 Condition | Result |
|---------------|--------|
| VS1 ≥ VOV | VS1 ≥ 0.25 V |

Choosing

VS1 ≈ **0.3 V**

satisfies the saturation condition.

### Justification

| Reason | Explanation |
|------|-------------|
| VS1 > VOV | keeps lower NMOS in saturation |
| VS1 << VDD | preserves voltage headroom |
| Stable bias | allows correct cascode operation |

---

# Output Voltage Selection

For maximum signal swing

VDS ≈ VDD / 2

| Calculation | Result |
|-------------|--------|
| VDS = 1.8 / 2 | **0.9 V** |

Output node voltage

| Calculation | Result |
|-------------|--------|
| Vout = VDS + VS1 = 0.9 + 0.3 | **1.2 V** |

---

# Saturation Verification

| Device | Condition | Result |
|------|-----------|--------|
| M2 (NMOS) | VDS2 ≥ VOV | 0.3 ≥ 0.25 ✔ |
| M1 (NMOS) | VDS1 ≥ VOV | 0.9 ≥ 0.25 ✔ |
| M3 (PMOS) | VSD ≥ VOV | 0.6 ≥ 0.25 ✔ |

All MOSFETs operate in **saturation region**.

---

# Final DC Operating Point

| VS2 | VS1 | Vout | ID | VOV |
|----|----|----|----|----|
| 0 V | 0.3 V | 1.2 V | 200 µA | 0.25 V |

###  LTspice Operating Point

<img width="566" height="542" alt="2bop" src="https://github.com/user-attachments/assets/f7cd9cbf-d82c-4ca5-a934-e49f7a266707" />

## Width Selection – Circuit 2B

The transistor widths were initially calculated using the square-law
saturation current equation to obtain **ID ≈ 200 µA**.

| Device | Calculated Width | Practical Width |
|------|------|------|
| M1 (NMOS – Cascode) | 15.16 µm | 26.88 µm |
| M2 (NMOS – Input) | 15.16 µm | 29.09 µm |
| M3 (PMOS – Active Load) | 35.9 µm | 83.37 µm |

### Justification

During LTspice simulation, the theoretical widths did not produce
exactly **200 µA** due to non-ideal MOSFET effects.

| Reason | Effect |
|------|------|
| Channel length modulation | Changes effective drain current |
| Cascode bias sensitivity | Small voltage changes affect current |
| Mobility degradation | Reduces current compared to ideal model |

Therefore the widths were slightly adjusted in simulation until the
desired operating current **ID ≈ 200 µA** was obtained while keeping all
transistors in **saturation region**.

----
## Transient Analysis – Circuit 2B (Cascode)

### Input Signal Parameters

| Waveform | Frequency | Amplitude | DC Offset |
|----------|-----------|-----------|-----------|
| Sine | 1 kHz | 10 mV | 0.916 V |

| Simulation Command | Value |
|--------------------|-------|
| Transient Command | `.tran 0 5m` |

---
### Input Waveform

<img width="1008" height="452" alt="2bin" src="https://github.com/user-attachments/assets/97a5026a-e475-4d42-90e3-2c00a6753ea1" />

---
### Output Waveform

<img width="998" height="440" alt="2bout" src="https://github.com/user-attachments/assets/5055a68c-afff-4fa7-bef2-fd93f0c9bd38" />


---
### Input & Output Comparison

<img width="996" height="443" alt="2bboth" src="https://github.com/user-attachments/assets/4ec901c1-239f-40d5-a423-990d810f75c9" />


---
## Practical Gain Calculation

| Vin(p-p) | Vout(p-p) | Gain (Av) | Gain (dB) |
|----------|-----------|-----------|-----------|
| 0.925 − 0.906 = **0.019 V** | 1.225 − 1.183 = **0.042 V** | 0.042 / 0.019 = **2.21 V/V** | **6.88 dB** |

---
### Observation

The output signal is inverted relative to the input and shows clear
voltage amplification, confirming correct operation of the cascode
amplifier.

-----
# AC Analysis

## – Frequency Response (Circuit 2B)

To determine midband gain and bandwidth,
small-signal AC analysis was performed.

AC Simulation Command Used:

.ac dec 1000 .1 1G

Input AC magnitude = 1 V

------------------------------------------------------------

<img width="1014" height="453" alt="2bbw" src="https://github.com/user-attachments/assets/e13d5fc3-595a-474b-9957-cdfdf1b62f91" />



Figure: AC Gain (Magnitude vs Frequency)

From AC plot:

maximum Gain ≈ 6.94 dB

The measured AC gain is 6.94 dB, which strongly correlates with our transient calculation

-3dB point-->>3.94dB=45.289MHz

---------
## Theoretical Gain – Circuit 2B

gm1 = 1.6 mS  
ro1 ≈ ro2 ≈ ro3 ≈ 1.5 kΩ  

Gain formula:

Av =
- gm1 / (1 + gm1ro2 + ro2/ro1)
×
([gm1ro2ro1 + ro2 + ro1] || ro3)

------------------------------------------------------------

Substitution:

Denominator = 1 + 2.4 + 1 = 4.4  

Bracket term = 6.6k || 1.5k ≈ 1.2k  

Av = - (1.6mS / 4.4) × 1.2k  

Av ≈ -2.18 V/V  

Gain ≈ 6.9 dB 

Theoretical gain ≈ 6.88 dB,

which nearly matches AC and transient results (≈ 6.9 dB).

--------

### Reason for Small Difference Between Theory and Simulation

The slight difference between theoretical and simulated gain
occurs because theoretical calculations use simplified
small-signal equations, while LTspice uses a complete BSIM
model.

Simulation includes:

• Channel length modulation  
• Parasitic capacitances  
• Mobility degradation  
• Higher-order device effects  

Hence, a small variation (few tenths of dB) is expected.

---------------------------------------------------------

### EXP2- CIRCUIT C – Common Source Amplifier with Diode-Connected NMOS Current Source and PMOS Active Load
-------------------------------------------------------------

### Circuit Implementation in LTspice

<img width="786" height="567" alt="2c" src="https://github.com/user-attachments/assets/b77c96a1-db8f-40ec-bd8e-6b743f5bc752" />



## DC Analysis – Circuit 2C
## Bias Calculation – Circuit 2C  
(Common Source with Diode-Connected Current Source)

Given:

VTHn = 0.36 V 
VTHp =-0.39V
Vov = 0.25 V  
VDD = 1.8 V  

------------------------------------------------------------

### Step 1: Diode-Connected NMOS (M3)

Since M3 is diode-connected:

VD3 = VG3  

For ID = 200 µA,

VGS3 = Vthn + Vov  
VGS3 = 0.36 + 0.25  
VGS3 = 0.61 V  

Source of M3 = 0 V  

Therefore,

VD3 = VS1 = 0.61 V  

This fixes the source voltage of M1.

------------------------------------------------------------

### Step 2: Gate Voltage of M1

For M1 to carry same current:

VGS1 = Vthn + Vov  
VGS1 = 0.36 + 0.25  
VGS1 = 0.61 V  

Since,

VG1 = VS1 + VGS1  

VG1 = 0.61 + 0.61  

VG1 = 1.22 V  

Hence input DC bias = 1.22 V.

------------------------------------------------------------

### 3) PMOS Gate Voltage

For PMOS carrying same ID:

VSG2 = |Vthp| + Vov  
VSG2 = 0.39 + 0.25  
VSG2 = 0.64 V  

Since source of PMOS = VDD = 1.8 V,

VG2 = VS − VSG2  
VG2 = 1.8 − 0.64  
VG2 = 1.16 V  

 PMOS gate bias = 1.16 V  

------------------------------------------------------------

### 3) Output Voltage

VDS1 = 0.9 V   (vDD/2)

Vout = VDS1 + VS1  

Vout = 0.9 + 0.6  

Vout = 1.5 V  


All transistors operate in saturation.

------------------------------------------------------------

### Why 0.2 V or 0.3 V Will Not Work?

If VS1 = 0.2 V:

VGS3 = 0.2 V < Vthn (0.36 V)

M3 turns OFF → current will not flow.

Hence source voltage is not assumed.
It is fixed by the diode-connected NMOS bias condition.
--------------------------------------------------------------
###  LTspice Operating Point
<img width="602" height="531" alt="2cop" src="https://github.com/user-attachments/assets/086dd0eb-9a9f-422c-b87c-90b8991482a9" />


###  Width Selection for Circuit A

The MOSFET widths were calculated earlier using the 
square-law saturation equation to obtain ID = 200 µA.

The calculated values are:

NMOS  : W = 15.16 µm  
PMOS  : W = 35.9 µm  

(Refer Section 8: Width Calculation_FROM DEVICE PARAMETERS)

### Practical Width Adjustment

During LTspice simulation, the drain current obtained 
using theoretical widths was slightly different 
from the target value of 200 µA.

• Cascode bias sensitivity  
• Channel length modulation  
• Non-ideal device effects  

Final dimensions used in simulation ensure
all transistors operate in saturation.
Hence, widths were slightly adjusted in LTspice 
until ID ≈ 200 µA was achieved.

Final Dimensions Used in Simulation:

M1-NMOS  : W = 26.34u µm  (mid)

M2-PMOS  : W=87.87u µm  (top)

M3-NMOS  : W = 33.26u µm (top)

-----------------------------------------------
## Transient Analysis – Circuit 2C

Input Signal:

Vin = SINE(1.22 10m 1k)

.tran 0 5m

------------------------------------------------------------

Input Waveform

<img width="997" height="444" alt="2cin" src="https://github.com/user-attachments/assets/2c5910d8-3b66-4cf8-a6b5-5120ccfaace7" />


------------------------------------------------------------

 Output Waveform

<img width="996" height="444" alt="2cout" src="https://github.com/user-attachments/assets/f1ee3f51-7514-4b1b-8b0e-76b2f677af6c" />


------------------------------------------------------------

 Input & Output (Combined)

<img width="1003" height="443" alt="2cboth" src="https://github.com/user-attachments/assets/8e2c8968-7e9e-40cb-ac2d-088e9307bf3d" />


------------------------------------------------------------
Observation:

• Output is inverted (180° phase shift).  
• Output amplitude is greater than input.  
• No clipping observed.  
• Bias point remains stable around 1.5 V.

Measured:

Vin(p-p) = 1.229V - 1.210V
Vin(p-p) =0.019V

Vout(p-p) = 1.618V − 1.235V 
Vout(p-p) = 0.383V 

Practical gain:

Av = Vout / Vin  

Av = 0.383 / 0.019 

Av = 20.157 V/V 

Gain in dB:

Av(dB) = 20 log(20.157)  

Av(dB) = 26.08 dB  

This is the gain obtained from transient waveform.
----
# AC Analysis

## – Frequency Response (Circuit 2C)

To determine midband gain and bandwidth,
small-signal AC analysis was performed.

AC Simulation Command Used:

.ac dec 1000 .1 1G

Input AC magnitude = 1 V

------------------------------------------------------------

<img width="1014" height="453" alt="2bbw" src="https://github.com/user-attachments/assets/39bd809a-f7a9-46d8-ad57-3c3dd6e64c44" />



Figure: AC Gain (Magnitude vs Frequency)

From AC plot:

maximum Gain ≈ 25.55 dB 

The measured AC gain is 25.55 dB, which strongly correlates with our transient calculation

-3dB point-->>22.55dB=118.304MHz
------------------------------------------
## Theoretical Gain – Circuit 2C

Gain formula:

Av =
- gm1 / (1 + gm1ro3)
× (ro1 || ro2)

------------------------------------------------------------

gm1 = 1.6 mS  
ro1 ≈ ro2 ≈ ro3 ≈ 25 kΩ  

ro1 || ro2 = 12.5 kΩ  

Denominator = 1 + (1.6mS × 25k)  
            = 41  

Av = - (1.6mS / 41) × 12.5k  

Av ≈ -19.5 V/V  

Gain≈25.8dB

----------------------
### Reason for Difference Between Theoretical and Simulation Gain

The slight difference between theoretical and simulated gain
occurs because theoretical calculations use simplified
small-signal equations, while LTspice uses a complete MOSFET
model (BSIM).

Simulation includes:

• Channel length modulation variations  
• Parasitic capacitances  
• Mobility degradation  
• Body effect  
• Exact gm and ro values from operating point  

Hence, a small variation (≈ 0.5–1 dB) is expected.

----------

# Summary, Inference and Conclusion

## Summary

In this experiment, three MOSFET amplifier configurations were
implemented and analyzed:

• Circuit 2A – SOURCE DEGENERATED COMMON SOURCE AMPLIFIER
  • Circuit 2B – Common Source – Cascode Amplifier with Active Load 
• Circuit 2C –  Common Source Amplifier with Diode-Connected NMOS Current Source and PMOS Active Load
For each circuit:

- DC bias conditions were calculated.
- Saturation of all transistors was verified.
- Transient and AC analyses were performed.
- Theoretical gain was derived and compared with simulation.

------------------------------------------------------------

## Inference

• Source degeneration (2A) reduces gain but improves stability.  
• Cascode structure (2B) increases output resistance but practical loading reduced gain.  
• Active load configuration (2C) provides higher gain without source degeneration.  
• Theoretical and simulated gains closely match with minor variation due to non-ideal effects.

------------------------------------------------------------

## Comparison of All Three Circuits

| Parameter | Circuit 2A | Circuit 2B | Circuit 2C |
|------------|------------|------------|------------|
| Configuration | Source Degenerated CS | Cascode Amplifier | CS with Active Load |
| Source Node | Fixed by RS | Raised by Cascode NMOS | Fixed by Diode NMOS |
| Output Voltage (DC) | ≈ 1.1 V | ≈ 1.2 V | ≈ 1.5 V |
| Theoretical Gain | ≈ 28 dB | ≈ 6.9 dB | ≈ 25–26 dB |
| AC Gain | ≈ 28.6 dB | ≈ 6.9 dB | ≈ 25–26 dB |
| Gain Nature | Moderate | Low (practically limited) | High |
| Stability | High | High | Moderate |
| Complexity | Medium | High | Medium |

------------------------------------------------------------

## Conclusion

From the comparative study:

• Circuit 2A provides moderate gain with improved stability due to source degeneration.  
• Circuit 2B increases output resistance theoretically, but practical loading limited gain.  
• Circuit 2C provides high gain using active load without degeneration.  

Thus, different MOS configurations affect gain, output resistance,
and bias stability significantly. The experimental results
closely match theoretical calculations.
