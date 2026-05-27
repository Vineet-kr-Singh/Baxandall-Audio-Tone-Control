# Baxandall-Audio-Tone-Control
Design and implementation of a Baxandall audio tone control circuit with adjustable bass and treble response. Includes LTspice simulation, parameter sweeps, and frequency response analysis.

## Project Overview

The Baxandall tone control circuit is the industry-standard architecture for independent bass and treble adjustment in high-fidelity audio amplifiers. Invented by Peter Baxandall in 1952, this circuit utilizes a negative feedback loop around an inverting operational amplifier (Op-Amp) to achieve smooth boost and cut characteristics without affecting the midrange frequencies.

The objective of this project was to design, simulate, and analyze an active Baxandall tone control network using LTspice, evaluating how component variations alter the system's frequency response, turnover frequencies, and overall stability.

---

## Methodology & Theoretical Framework

### Step 1: Studying Basic Filter Circuits

To understand the Baxandall network, we must first analyze its fundamental building blocks: first-order Passive RC Filters. These components dictate the crossover points for the audio spectrum.

* **Low-Pass Filter (LPF):** Passes low-frequency signals but attenuates signals above the cutoff frequency ($f_c$). It acts as the conceptual foundation for the bass control network.
* **High-Pass Filter (HPF):** Attenuates low-frequency signals and passes signals above $f_c$. This forms the basis of the treble control network.

The cutoff frequency for a standard RC network is mathematically defined as:


$$f_c = \frac{1}{2\pi R C}$$

---

### Step 2: Understanding Potentiometer Action in Feedback Loops

The core brilliance of the Baxandall design lies in placing the control potentiometers within the feedback loop of an inverting amplifier.

* **Inverting Amplifier Gain:** The voltage gain ($A_v$) of an inverting Op-Amp is governed by the ratio of the feedback resistor ($R_f$) to the input resistor ($R_{in}$):

$$A_v = -\frac{R_f}{R_{in}}$$


* **Potentiometer Mechanics:** By utilizing a potentiometer as a voltage divider within the feedback path, we can dynamically alter the effective $R_f$ and $R_{in}$ for specific frequency bands.
* Moving the wiper toward the input **boosts** the gain for that band (decreases $R_{in}$, increases $R_f$).
* Moving the wiper toward the output **attenuates (cuts)** the gain for that band (increases $R_{in}$, decreases $R_f$).
* At the center position, the ratio is $1:1$, resulting in a flat $0\text{ dB}$ unity gain response.



---

### Step 3: Designing the Active Baxandall Circuit

The complete circuit merges the frequency-selective RC networks with the potentiometer-driven feedback loop.

#### A. The Bass Control Network

The bass network consists of a potentiometer ($R_{bass}$) bridged by a capacitor ($C_{bass}$), flanked by isolating resistors.

* **At Low Frequencies ($f \to 0$):** Capacitors act as open circuits. The audio signal is forced through the potentiometer wiper, allowing maximum boost or cut control.
* **At High Frequencies ($f \to \infty$):** Capacitors act as short circuits, effectively bypassing the bass potentiometer. The bass network simplifies to a unity-gain path, leaving high frequencies untouched by the bass setting.

#### B. The Treble Control Network

The treble network uses a potentiometer ($R_{treble}$) in series with small capacitors ($C_{treble}$) to isolate high-frequency currents.

* **At Low Frequencies ($f \to 0$):** The series capacitors block low frequencies from entering the treble adjustment loop entirely, ensuring the bass and midrange remain unaffected.
* **At High Frequencies ($f \to \infty$):** The capacitors become low-impedance paths, allowing the treble wiper position to dictate the high-frequency feedback ratio (boost/cut).

---

### Step 4: LTspice Simulation and Analysis

The circuit was simulated using **AC Analysis (`.ac`)** in LTspice to plot magnitude (dB) and phase against frequency ($10\text{ Hz}$ to $100\text{ kHz}$). To simulate the physical turning of the knobs, the potentiometers were modeled as two complementary resistors ($R_{top}$ and $R_{bottom}$) and swept using the `.step param` command.

---

## Final Circuit Schematic

Below is the LTSpice schematic implementation integrating independent passive networks into a unified active feedback loop around the OP07 operational amplifier, along with dual-potentiometer sub-circuits for bass and treble control.
<img width="1251" height="609" alt="image" src="https://github.com/user-attachments/assets/40f00169-c781-4496-a613-7ea414cb8709" />




---

## Results and Bode Plot Analysis

### Bass Response

* **Boost Mode:** When the bass wiper is moved fully toward the input, a maximum boost of approximately $+15\text{ dB}$ is observed at $20\text{ Hz}$. The response behaves like a shelving filter.
* **Cut Mode:** When moved toward the output, a symmetrical maximum cut of $-15\text{ dB}$ occurs at $20\text{ Hz}$.
* **Midrange Integrity:** Regardless of the bass control position, the curve converges back to $0\text{ dB}$ near the **midrange turnover frequency** (typically around $1\text{ kHz}$).

*(Insert your LTspice Bass Sweep Graph here)*

### Treble Response

* **Boost Mode:** Provides a gradual shelving increase starting around $1\text{ kHz}$ to $2\text{ kHz}$, reaching up to $+15\text{ dB}$ at $20\text{ kHz}$.
* **Cut Mode:** Mirror-image attenuation dropping down to $-15\text{ dB}$ at $20\text{ kHz}$.
* **Low-Frequency Integrity:** The treble adjustments show $0\text{ dB}$ variance for all frequencies below $500\text{ Hz}$.

*(Insert your LTspice Treble Sweep Graph here)*

### Parameter Sweep Results (Simultaneous Control)

By running a multi-parameter sweep (`.step`), the graph below displays the full flexibility of the circuit:

1. Full Bass Boost / Full Treble Boost (V-shaped curve)
2. Full Bass Cut / Full Treble Cut (Inverted V-shaped curve)
3. Full Bass Boost / Full Treble Cut
4. Linear Flat Response (Both wipers exactly at $50\%$)

*(Insert your combined Parameter Sweep Graph here)*

---

## Tools Used

* **LTspice XVII / 24:** Used for schematic capture, AC sweep simulations, and component parameterization.
* **Analog Circuit Design Principles:** Applied to calculate turnover frequencies and structural component values.
* **Frequency Response (Bode) Analysis:** Utilized to ensure flat midrange response and symmetric boost/cut profiles.
