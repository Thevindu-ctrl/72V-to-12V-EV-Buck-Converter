# 72V-to-12V-EV-Buck-Converter (Vin 60-90)

Data-driven mathematical design, closed-loop control system architecture, and dynamic transient simulation of a high-efficiency synchronous DC-DC Buck Converter desinged for Electric Vehicle (EV) accessory power rails.

---


![MATLAB](https://img.shields.io/badge/MATLAB-R2024b-orange?style=for-the-badge&logo=mathworks)
![Simulink](https://img.shields.io/badge/Simulink-Simulation-blue?style=for-the-badge&logo=mathworks)
![Category](https://img.shields.io/badge/Domain-Power_Electronics-red?style=for-the-badge)


---

##Overview

This project focuses on the complete mathematical sizing and behavioral simulation of a DC-DC Buck Converter designed to step down a highly variable electric vehicle input voltage array down to a stable, low-voltage rail dedicated to vehicle accessories (such as lighting, infotainment, and telemetry modules). 

Modeled completely within **MATLAB & Simulink**, the circuit features an advanced closed-loop control strategy. It combines a dynamic feedforward duty cycle calculation with a tuned **Proportional-Integral (PI) Controller** to suppress switching ripples and ensure fast, stable recovery from extreme input line disturbances.

---

## Technical Specifications & Component Parameters

The system components were mathematically calculated and optimized to enforce **Continuous Conduction Mode (CCM)** under heavy load profiles, preventing discontinuous voltage drop-offs:

| Parameter / Component | Specified Value | Units | Description / Hardware Constraints |
| :--- | :---: | :---: | :--- |
| **Input Voltage ($V_{in}$)** | 60 – 90 | V | Simulates a highly variable, unregulated EV battery pack nominal rail (72V base) |
| **Output Voltage ($V_{out}$)** | 12 | V | Regulated accessory rail targeting precision 12V delivery |
| **Output Current ($I_{out}$)** | Up to 10 | A | Maximum current capacity delivered to the subsystem |
| **Load Resistance ($R_{load}$)** | 1.2 | $\Omega$ | Standard continuous operational accessory impedance load |
| **Switching Frequency ($f_{sw}$)** | 100,000 | Hz | High-speed gate switching frequency for reduced physical component volume |
| **Sample Time** | $1 \times 10^{-6}$ | s | Precision solver discrete step clock rate ($1\,\mu\text{s}$) |
| **Timer Period** | $10 \times 10^{-6}$ | s | Discrete step timing cycle allocation ($10\,\mu\text{s}$) |
| **Inductor ($L$)** | 4 | mH | Designed for suppression of operational current ripples |
| **Inductor Resistance ($R_L$)** | 1 | $\Omega$ | Internal parasitic DC resistance of the copper winding |
| **Capacitor ($C$)** | 220 | $\mu\text{F}$ | Output low-pass filter network to dampen high-frequency switching noise |
| **Freewheeling Diode ($V_f$)** | 0.9 | V | Forward voltage drop across the recirculation diode |

---

## losed-Loop Control Architecture

The system utilizes a multi-stage closed-loop system block to dynamically achieve line and load regulation:

[Vout Feedback] ────> ( Error Block: Vref - Vout ) ────> [ Tuned PI Controller ]
│
▼
[Vin Feedforward] ──> [ Duty Cycle Normalizer (1 / Vin) ] ──> ( Combiner )
│
▼
[MOSFET Gate] <─── [ 100kHz Sawtooth Comparator ] <─── [ Saturation Filter (0 to 1) ]


### Core Logic 
1. **Dynamic Feedforward Calculation:** Rather than relying solely on feed-back loops, the control structure divides the error-corrected voltage vector by the live measured input voltage ($V_{in}$). This preemptively offsets massive input fluctuations (such as jumping instantly from 60V to 90V) before they hit the output rail.
2. **Proportional-Integral (PI) Loop:** Continuously corrects minor steady-state voltage deviations from the exact 12V target.
3. **Saturation Filter:** Clamps the resulting duty cycle signal strictly between `0.0` and `1.0` to eliminate integral windup or driver damage.
4. **100kHz PWM Generation:** Compares the normalized duty cycle signal directly against a high-frequency sawtooth carrier sequence to modulate the power MOSFET's gate state (`IF D > SAWTOOTH -> MOSFET ON; ELSE -> MOSFET OFF`).

---

## Repository Structure

```text
72V-to-12V-EV-Buck-Converter/
├── simulation/
│   └── BuckConverter_PowerCircuit.slx  # Core MATLAB/Simulink schematic model file
└── docs/
    └── Powerelecminiproj.pdf           # Verified engineering project report with embedded graphs
```


## How to Run the Simulation

Clone the Repository:

```text
Bash
git clone [https://github.com/Thevindu-ctrl/72V-to-12V-EV-Buck-Converter.git](https://github.com/Thevindu-ctrl/72V-to-12V-EV-Buck-Converter.git)
```
Launch MATLAB: Open MATLAB / Simulink (Version R2024b or newer recommended).

Open the Model: Navigate to the simulation/ directory and open BuckConverter_PowerCircuit.slx.

Run Simulation: Click the blue Run button on the Simulink toolstrip to initiate the solver.

Analyze Waveforms: To view the verified analytical scopes, step response graphs, duty cycle tracking, and academic calculations, refer to the full technical document located under docs/Powerelecminiproj.pdf.

## Author Credits
Developer: W.L.P.T.N. Wijayasinghe 


