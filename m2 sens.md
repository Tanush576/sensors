<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

## BEEE412L – Sensors and Actuators

### Module 2 – Static \& Dynamic Characteristics and Transducer Dynamics


***

## 1. Static Characteristics of Sensors

Static characteristics describe the behavior of a sensor when the input is constant or varies very slowly.[^1]

### 1.1 Key Static Characteristics

- **Accuracy**
    - Indicates closeness of measured value to the true value.[^1]
    - Often expressed as maximum error as a percentage of full-scale reading. ⭐
    - Example formula:
        - Accuracy $\approx \dfrac{\text{max error}}{\text{full scale}} \times 100 \%$.[^1]
- **Precision**
    - Indicates **repeatability** or reproducibility of measurements under identical conditions.[^1]
    - A precise instrument gives closely grouped readings, even if they are all offset from the true value.
- **Accuracy vs Precision** ⭐
    - High accuracy, high precision: readings close to each other and to true value.[^1]
    - Low accuracy, high precision: readings close to each other but far from true value.[^1]
    - Low accuracy, low precision: readings scattered and away from true value.[^1]
- **Nonlinearity**
    - Deviation of the actual input–output curve from an ideal straight line.[^1]
    - Defined as maximum deviation from the linear characteristic as a percentage of full-scale output.[^1]
- **Resolution**
    - Minimum change in input that can be detected.[^1]
    - Describes the smallest increment of stimulus that causes a detectable output change.[^1]
- **Sensitivity**
    - Ratio of incremental output to incremental input (slope of static characteristic).[^1]
    - For approximately linear sensors, sensitivity is constant over the range.[^1]
    - Example: Spring balance sensitivity = 25 mm/kg ⇒ 1 kg causes 25 mm displacement.[^1]
- **Repeatability**
    - Inability of the sensor to give the same output for repeated measurements under identical conditions.[^1]
    - Quantified as maximum difference between readings over two measurement cycles, usually as
$\text{Repeatability error} = \dfrac{r}{\text{FS}} \times 100 \%$.[^1]
- **Hysteresis**
    - Difference in output for the same input when approached from increasing vs decreasing side.[^1]
    - Caused by mechanical play, magnetic effects, material properties, etc. ⚠️
- **Range / Span / Full-Scale Input**
    - Dynamic range of the measurand that the sensor can convert acceptably.[^1]
    - Span: difference between maximum and minimum recommended input (or output).[^1]
    - Full-scale output: algebraic difference between output at maximum and minimum input.[^1]
- **Output Impedance**
    - Important for interfacing sensor to electronic circuits.[^1]
    - Current-type sensors: prefer high output impedance and low load input impedance.[^1]
    - Voltage-type sensors: prefer low output impedance and high circuit input impedance.[^1]


### 1.2 Accuracy Example – Pressure Gauges

Two pressure gauges with ±5% full-scale accuracy:[^1]

- Gauge A: range 0–1 bar
    - Max error = $5\% \times 1 = 0.05$ bar.[^1]
    - At reading 0.9 bar, percentage error $\approx \dfrac{0.05}{0.9} \times 100 \approx 5.6\%$.[^1]
- Gauge B: range 0–10 bar
    - Max error = $5\% \times 10 = 0.5$ bar.[^1]
    - At reading 0.9 bar, percentage error $\approx \dfrac{0.5}{0.9} \times 100 \approx 55\%$.[^1]

⭐ Conclusion: Gauge A is more suitable at 0.9 bar due to much lower percentage error.[^1]

***

## 2. Dynamic Characteristics of Measurement Systems

Dynamic characteristics describe how an instrument behaves when the input changes rapidly with time.[^1]

### 2.1 Basic Dynamic Terms

- **Speed of Response**
    - Rapidity with which the system responds to a change in input.[^1]
- **Response Time**
    - Time required for the output to settle to its final steady value after a change in input.[^1]
- **Lag**
    - Delay between change in input and corresponding change in output.[^1]
- **Fidelity**
    - Ability of the system to reproduce output in the same form as the input (shape preserved).[^1]
- **Dynamic Error**
    - Difference between true value of a time-varying quantity and the indicated value, assuming zero static error.[^1]

💡 Intuition: Static characteristics = “how good is a photograph of one instant?”; dynamic characteristics = “how faithfully can we capture a fast-moving video?”.

### 2.2 General Dynamic Model and Order of System

A linear time-domain model for a measurement system is often expressed as a differential equation relating output $x_o$ and input $x_i$:[^1]

$$
a_n \dfrac{d^n x_o}{dt^n} + a_{n-1} \dfrac{d^{n-1} x_o}{dt^{n-1}} + \dots + a_1 \dfrac{dx_o}{dt} + a_0 x_o
=
b_m \dfrac{d^m x_i}{dt^m} + \dots + b_0 x_i
$$

- The order of the system is the highest derivative in the equation, equal to the number of energy storage elements.[^1]
- Energy storing elements: mass, inductance, capacitance, thermal capacitance, fluid inertia, etc.[^1]
- Common sensor dynamics: zero-order, first-order, second-order.[^1]

Using Laplace transforms, a transfer function $G(s) = \dfrac{X_o(s)}{X_i(s)}$ is obtained.[^1]

***

## 3. Classification of Transducers by Order

### 3.1 Zero-Order Transducers

When all coefficients except the proportional term are zero, equation reduces to a static relation:[^2]

$$
x_o = K x_i
$$

- $K$ is the **static sensitivity**.[^2]
- Output follows input perfectly with no time lag and no distortion.[^2]
- Represents ideal dynamic performance (infinite speed of response). ⭐[^2]

**Example: Displacement Potentiometer**[^2]

- For a linear potentiometer of total length $L$ and supply voltage $V_s$:
    - Output voltage $V_o = V_s \dfrac{x}{L}$.[^2]
    - Static sensitivity $K = \dfrac{V_s}{L}$.[^2]

💡 Analogy: Like an ideal ruler – instant reading, no delay, purely proportional.

### 3.2 First-Order Transducers

If only first derivative and zero-order term are present:[^2]

$$
\tau \dfrac{dx_o}{dt} + x_o = K x_i
$$

- $K$: static sensitivity; $\tau$: time constant of system.[^2]
- Transfer function:

$$
G(s) = \dfrac{K}{\tau s + 1}
$$[^2]

**Example: Thermocouple in a Fluid Bath**[^2]

- Heat balance for sensing bead:[^2]
    - $Q$: overall heat-transfer coefficient
    - $A$: heat transfer area
    - $T_1$: bead temperature (indicated)
    - $T_2$: fluid temperature
    - $M$: bead mass
    - $S$: specific heat
- Heat balance leads to first-order differential equation of $T_1$ vs $T_2$.[^2]
- Thermocouple output: $V = K (T_{hot} - T_{cold})$.[^2]
    - With cold junction at $0^\circ C$, $V \propto T_1$.[^2]
- Overall transfer function is first order ⇒ thermocouple behaves as a first-order transducer.[^2]

💡 Intuition: Like a “leaky bucket” filling with water; it approaches final level gradually with characteristic time constant $\tau$.

### 3.3 Second-Order Transducers (Standard Model)

Standard second-order transfer function:[^3][^2]

$$
G(s) = \dfrac{\omega_n^2}{s^2 + 2 \zeta \omega_n s + \omega_n^2}
$$

- $\omega_n$: natural frequency (rad/s)
- $\zeta$: damping ratio
- System type depends on $\zeta$:
    - $\zeta > 1$: overdamped
    - $\zeta = 1$: critically damped
    - $0 < \zeta < 1$: underdamped
    - $\zeta = 0$: undamped

⭐ Many inertial sensors (accelerometers, vibration/transient sensors) exhibit second-order dynamics.[^3][^2]

***

## 4. Standard Responses of First- and Second-Order Systems

### 4.1 First-Order System Responses

Transfer function: $G(s) = \dfrac{K}{\tau s + 1}$.[^2][^1]

- **Step Input** (e.g., sudden temperature change):
    - Time response:

$$
y(t) = K \, u_0 \left(1 - e^{-t/\tau}\right)
$$

where $u_0$ is step size.
    - Output rises exponentially and asymptotically approaches final value.[^3][^2]
    - After $t = \tau$, response reaches about 63.2% of final value.
- **Sinusoidal Input**
    - Output sinusoid has same frequency but attenuated amplitude and phase lag.[^2]
    - High frequencies ⇒ larger attenuation and phase lag. ⚠️


### 4.2 Second-Order System Responses

Standard second-order system exhibits different responses based on input type.[^2]

- **Impulse Input**
    - Output is oscillatory and exponentially decaying; ends at zero.[^2]
- **Step Input**
    - Underdamped response: oscillatory, exhibits overshoot, and settles to final value 1 (normalized).[^2]
    - Overdamped: sluggish, no oscillations.
- **Ramp Input**
    - Output lags behind input with initial oscillations and increasing steady-state error.[^2]
- **Sinusoidal Input**
    - Output has same frequency as input.[^2]
    - Amplitude varies with frequency; phase lag present.[^2]
    - Resonance possible when damping ratio is small (near $\zeta \approx 0$).[^2]


### 4.3 Response Parameters (Mostly Second-Order)

- **Peak Overshoot** ($M_p$)
    - Deviation of the response at peak time from the final value; also called maximum overshoot.[^1]
- **Peak Time** ($t_p$)
    - Time required for response to reach first peak value.[^1]
- **Rise Time** ($t_r$)
    - For underdamped systems: time from 0 to 100% of final value.[^1]
    - For overdamped: usually 10–90% of final value.[^1]
- **Settling Time** ($t_s$)
    - Time for output to enter and remain within specified tolerance band (e.g., ±2%, ±5%) around final value.[^1]

💡 Exam tip: Always specify the band (2% or 5%) when quoting settling time.

***

## 5. Example Problems (First-Order Systems)

### 5.1 Mercury Thermometer (First-Order)

- A mercury thermometer modeled as first-order with time constant $\tau = 10$ s is subjected to a sudden temperature change from 20°C to 100°C.[^3]
- Step magnitude: $80^\circ C$.
- Reading after 10 s:
    - $T(t) = T_{final} - (T_{final} - T_{initial}) e^{-t/\tau}$.
    - At $t = \tau$: thermometer indicates about 63.2% of total change ⇒ roughly $20 + 0.632 \times 80 \approx 70.6^\circ C$.

⭐ Method: Identify step size, time constant, apply standard first-order step response.

### 5.2 Liquid Level Sensor

- Transfer function given (first-order) with time constant $\tau = 5$ s.[^3]
- For unit step in liquid level:
    - Time constant directly read from denominator term.
    - Output after 5 s given by first-order step response formula:

$$
y(5) = 1 - e^{-5/\tau} = 1 - e^{-1}
$$

(for unit final value, normalized).[^3]

***

## 6. Example Problems (Second-Order Systems)

### 6.1 Accelerometer Model

- Accelerometer modeled with a differential equation comparable to standard second-order form.[^3]
- Procedure:
    - Rewrite equation in standard form:

$$
\dfrac{d^2 x}{dt^2} + 2\zeta \omega_n \dfrac{dx}{dt} + \omega_n^2 x = \omega_n^2 u(t)
$$
    - Extract natural frequency $\omega_n$ and damping ratio $\zeta$.[^3]
    - If $0 < \zeta < 1$: system is underdamped (oscillatory response).[^3]


### 6.2 Seismic Sensor – Peak Overshoot

- Seismic sensor with damping ratio $\zeta = 0.2$.[^3]
- Percentage peak overshoot $M_p$ (standard control formula):

$$
M_p = e^{\frac{-\pi \zeta}{\sqrt{1-\zeta^2}}} \times 100\%
$$
- With $\zeta = 0.2$, overshoot is significant (under-damped).[^3]


### 6.3 Vibration Sensor – Settling Time

- Vibration sensor: $\zeta = 0.6$, $\omega_n = 20$ rad/s.[^3]
- 2% criterion settling time (standard):

$$
t_s \approx \dfrac{4}{\zeta \omega_n}
$$
- Substitute $\zeta = 0.6, \omega_n = 20$ to obtain numerical value.[^3]

💡 Tip: Memorize key second-order formulas: $M_p$, $t_p$, $t_s$ for quick exam calculations.

***

## 7. Useful Visual/ASCII Representations

### 7.1 Static vs Dynamic Characteristics

- Static:
    - Input $\rightarrow$ Sensor $\rightarrow$ Output
    - (Very slow or constant input)
- Dynamic:
    - Time-varying Input $x_i(t)$
↓
[Energy storage + dissipation]
↓
Output $x_o(t)$ with lag, distortion


### 7.2 Order Classification Flow

- Start → Write dynamic equation
    - If only proportional term → Zero order
    - If first derivative + proportional → First order
    - If second derivative term present → Second order

***

## 8. Quick Revision Sheet (High Yield)

- Static characteristics: accuracy, precision, resolution, sensitivity, nonlinearity, hysteresis, repeatability, range/span, output impedance.[^1]
- Accuracy: closeness to true value; often % of full-scale.[^1]
- Precision: repeatability; does not ensure accuracy.[^1]
- Nonlinearity: max deviation from ideal straight line as %FS.[^1]
- Resolution: smallest detectable change in input.[^1]
- Sensitivity: $\Delta \text{output} / \Delta \text{input}$; slope of static characteristic.[^1]
- Hysteresis: difference in outputs for same input when approached from increasing vs decreasing side.[^1]
- Range/Span: recommended operating input limits; FSO = output(max) – output(min).[^1]
- Dynamic characteristics: speed of response, response time, lag, fidelity, dynamic error.[^1]
- Zero-order: $y = Kx$; ideal, no time lag; example: ideal potentiometer.[^2]
- First-order: $\tau \dfrac{dy}{dt} + y = Kx$; transfer $K/(\tau s + 1)$; example: thermocouple.[^2]
- Second-order: $\omega_n^2 / (s^2 + 2 \zeta \omega_n s + \omega_n^2)$; inertial sensors.[^3][^2]
- Step response first-order: $y(t) = K u_0 (1 - e^{-t/\tau})$.[^3]
- Key second-order metrics: $M_p$, $t_p$, $t_r$, $t_s$.[^1]

***

## 9. Exam-Focused Cheat Sheet

⭐ **Static**

- Accuracy $\% = \dfrac{\text{max error}}{\text{FS}} \times 100$.[^1]
- Resolution → minimum detectable $\Delta$input.[^1]
- Sensitivity $S = \dfrac{\Delta y}{\Delta x}$.[^1]
- Nonlinearity $\%FS = \dfrac{\text{max deviation}}{\text{FS}} \times 100$.[^1]

✔ **Dynamic models**

- Zero-order: $y = Kx$.[^2]
- First-order: $\tau \dfrac{dy}{dt} + y = Kx$.[^2]
- Second-order: $\dfrac{d^2 y}{dt^2} + 2 \zeta \omega_n \dfrac{dy}{dt} + \omega_n^2 y = \omega_n^2 x$.[^3]

✔ **First-order response**

- Step: $y(t) = K u_0 (1 - e^{-t/\tau})$.
- After $t = \tau$: 63.2% of final value.

✔ **Second-order (standard formulas)**

- Step overshoot: $M_p = e^{-\pi \zeta / \sqrt{1-\zeta^2}} \times 100\%.$[^3]
- Peak time: $t_p = \dfrac{\pi}{\omega_n \sqrt{1-\zeta^2}}$.
- Settling time (2%): $t_s \approx \dfrac{4}{\zeta \omega_n}$.[^3]

⚠️ Always check if system is underdamped ($0 < \zeta < 1$) before using overshoot and $t_p$ formulas.

***

## 10. Important Exam-Oriented Q\&A (30)

1. **Define accuracy and precision.**
    - Accuracy is closeness of measured value to true value, usually expressed as % of full scale.[^1]
    - Precision is the degree of repeatability of measurements under identical conditions.[^1]
2. **Can an instrument be precise but not accurate?**
    - Yes; readings may be tightly clustered (precise) but consistently offset from true value (inaccurate).[^1]
3. **What is nonlinearity in sensors?**
    - Maximum deviation of actual static characteristic from an ideal straight line, expressed as %FS.[^1]
4. **Define resolution.**
    - The minimum change in input that the sensor can detect.[^1]
5. **Define sensitivity with example.**
    - Sensitivity is ratio of change in output to change in input.[^1]
    - Example: spring balance 25 mm/kg ⇒ 1 kg causes 25 mm displacement.[^1]
6. **Differentiate accuracy and precision with a simple statement.**
    - Accuracy: closeness to true value; precision: closeness of repeated measurements to each other.[^1]
7. **What is hysteresis in a sensor?**
    - Difference in sensor output for same input when approached from increasing vs decreasing direction.[^1]
8. **Define range and span.**
    - Range: minimum to maximum measurable input; span: difference between these two.[^1]
9. **What is full scale output?**
    - Difference between sensor outputs at maximum and minimum inputs.[^1]
10. **Why is output impedance important for sensor interfacing?**
    - It determines proper matching with circuit input impedance to avoid loading and signal loss.[^1]
11. **List the main dynamic characteristics of instruments.**
    - Speed of response, response time, lag, fidelity, dynamic error.[^1]
12. **Define speed of response and response time.**
    - Speed of response: rapidity of instrument response to input change.[^1]
    - Response time: time taken to settle at final steady value after change.[^1]
13. **What is measuring lag?**
    - Delay between a change in measured quantity and change in instrument output.[^1]
14. **Define fidelity.**
    - Ability to reproduce output in the same form as the input signal.[^1]
15. **What is dynamic error?**
    - Difference between true time-varying value and indicated value when static error is assumed zero.[^1]
16. **State the general role of system order in sensor dynamics.**
    - System order equals number of energy storage elements and influences response speed and shape.[^1]
17. **Give the definition of a zero-order transducer.**
    - A transducer whose output is directly proportional to input without time lag, modeled as $y = Kx$.[^2]
18. **Give one example of a zero-order transducer.**
    - Displacement measuring potentiometer assuming pure resistance and linearity.[^2]
19. **Provide the standard first-order differential equation for a transducer.**
    - $\tau \dfrac{dy}{dt} + y = Kx$.[^2]
20. **What physical quantities define the time constant in a thermocouple?**
    - Mass $M$, specific heat $S$, and overall heat transfer parameters $Q$, $A$ of the sensing bead.[^2]
21. **Why is a thermocouple modeled as first-order?**
    - Because its heat balance equation results in a first-order differential equation relating bead temperature to fluid temperature.[^2]
22. **What is a standard second-order system transfer function?**
    - $G(s) = \dfrac{\omega_n^2}{s^2 + 2 \zeta \omega_n s + \omega_n^2}$.[^3]
23. **Define natural frequency and damping ratio.**
    - Natural frequency $\omega_n$: frequency of undamped oscillations.[^3]
    - Damping ratio $\zeta$: measure of damping relative to critical damping.[^3]
24. **Classify a second-order system based on damping ratio.**
    - $\zeta > 1$: overdamped; $\zeta = 1$: critically damped; $0 < \zeta < 1$: underdamped; $\zeta = 0$: undamped.[^3]
25. **What is percentage peak overshoot?**
    - Maximum amount by which response exceeds final value, expressed as % of final value.[^1]
26. **How does damping ratio affect overshoot?**
    - Lower damping ratio leads to higher overshoot; with $\zeta \ge 1$, overshoot disappears.[^3][^1]
27. **What is settling time and on what does it depend?**
    - Time for response to enter and stay within a tolerance band around final value; depends on $\zeta$ and $\omega_n$.[^1]
28. **Describe the step response of a first-order system.**
    - Monotonic exponential rise from initial to final value, with characteristic time constant $\tau$.[^3][^2]
29. **Describe the step response of an underdamped second-order system.**
    - Oscillatory response with overshoot and exponential decay to final value.[^2]
30. **Why are first- and second-order models widely used for sensors?**
    - Most practical sensor dynamics can be approximated by these orders, simplifying analysis and design while retaining essential behavior.[^2][^1]

<div align="center">⁂</div>

[^1]: Module-2.pptx

[^2]: Module-2-second-half.pptx

[^3]: Module-2-problems.pptx

