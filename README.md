# Control Design of VSC with LCL Filter (dq Frame)

---

### I. System Description

A three-phase voltage source converter (VSC) is connected to the grid through an LCL filter. The system parameters are:
- $L_f, R_f$: inverter-side inductance and resistance  
- $C_f$: filter capacitance  
- $L_c, R_c$: grid-side inductance and resistance  

Control design focuses on:
- $L_f, R_f$ for current loop  
- $C_f$ for voltage loop
---

### II. Assumptions

1. Balanced three-phase system  
2. dq frame aligned with grid voltage  
3. Inner loop bandwidth ≫ outer loop   

---

### III. dq Model of the System (Before Decoupling)

#### A. Inverter-Side Inductor Dynamics

In dq frame, the inductor dynamics include cross-coupling terms:

$$
L_f \frac{di_d}{dt} = v_{cd} - v_d - R_f i_d + \omega L_f i_q
$$

$$
L_f \frac{di_q}{dt} = v_{cq} - v_q - R_f i_q - \omega L_f i_d
$$

where:

- $i_d, i_q$: dq-axis currents  
- $v_{cd}, v_{cq}$: converter output voltages  
- $v_d, v_q$: PCC voltages  
- $\omega$: grid angular frequency

---

#### B. Capacitor Dynamics

$$
C_f \frac{dv_d}{dt} = i_d - i_{gd} + \omega C_f v_q
$$

$$
C_f \frac{dv_q}{dt} = i_q - i_{gq} - \omega C_f v_d
$$

---

#### C. Grid-Side Inductor

$$
L_c \frac{di_{gd}}{dt} = v_d - v_{gd} - R_c i_{gd} + \omega L_c i_{gq}
$$

$$
L_c \frac{di_{gq}}{dt} = v_q - v_{gq} - R_c i_{gq} - \omega L_c i_{gd}
$$

---

### IV. Decoupling and Simplified Model

The cross-coupling terms $\omega L_f i_q$ and $\omega L_f i_d$ are compensated in control.

$$
v_{cd}^\ast = v_{cd} - \omega L_f i_q
$$

$$
v_{cq}^\ast = v_{cq} + \omega L_f i_d
$$

After decoupling, dynamics reduce to:

$$
L_f \frac{di}{dt} + R_f i = v_c
$$

Thus the plant becomes:

$$
G_i(s) = \frac{1}{L_f s + R_f}
$$

---

### V. Power Equations

$$
P = \frac{3}{2}(v_d i_d + v_q i_q)
$$

$$
Q = \frac{3}{2}(v_q i_d - v_d i_q)
$$

With voltage alignment:

$$
v_q \approx 0
$$

$$
P \approx \frac{3}{2} v_d i_d
$$

$$
Q \approx -\frac{3}{2} v_d i_q
$$

---

### VI. Current Loop Design

#### A. Plant

$$
G_i(s) = \frac{1}{L_f s + R_f}
$$

---

#### B. Controller

$$
C_i(s) = K_{pc} + \frac{K_{ic}}{s}
$$

---

#### C. Gain Selection

Cancel plant pole:

$$
\frac{K_{ic}}{K_{pc}} = \frac{R_f}{L_f}
$$

Choose bandwidth:

$$
\omega_{ci}
$$

Then:

$$
K_{pc} = L_f \omega_{ci}
$$

$$
K_{ic} = R_f \omega_{ci}
$$

---

#### D. Closed-Loop

$$
L_i(s) = \frac{\omega_{ci}}{s}
$$

$$
\frac{i(s)}{i^\ast(s)} = \frac{\omega_{ci}}{s + \omega_{ci}}
$$

---

### VII. Voltage Loop Design

#### A. Effective Plant

$$
C_f \frac{dv}{dt} = i
$$

$$
\frac{i(s)}{i^\ast(s)} = \frac{\omega_{ci}}{s + \omega_{ci}}
$$

$$
G_v(s) = \frac{\omega_{ci}}{(s + \omega_{ci})} \cdot \frac{1}{C_f s}
$$

---

#### B. Controller

$$
C_v(s) = K_{pv} + \frac{K_{iv}}{s}
$$

$$
\omega_{cv} = \frac{K_{iv}}{K_{pv}}
$$

---

#### C. Phase Margin Condition

$$
\angle L_v(j\omega) = \tan^{-1}\left(\frac{\omega}{\omega_{cv}}\right) - \tan^{-1}\left(\frac{\omega}{\omega_{ci}}\right)-180^\circ
$$

At crossover:

$$
pm =\tan^{-1}\left(\frac{\omega_{gc}}{\omega_{cv}}\right)-\tan^{-1}\left(\frac{\omega_{gc}}{\omega_{ci}}\right)
$$

---

#### D. Symmetric Design

$$
\omega_{gc} = \sqrt{\omega_{cv} \cdot \omega_{ci}}
$$

Let:

$$
a = \sqrt{\frac{\omega_{cv}}{\omega_{ci}}}
$$

$$
pm = 90^\circ - 2\tan^{-1}(a)
$$

$$
a = \tan\left(\frac{90^\circ - pm}{2}\right)
$$

$$
\frac{\omega_{cv}}{\omega_{ci}} = a^2
$$

$$
\boxed{\omega_{cv}=\omega_{ci}\frac{1 - \sin(pm)}{1 + \sin(pm)}}
$$

---

#### E. Gains

$$
K_{pv} = C_f \omega_{gc}
$$

$$
K_{iv} = K_{pv} \omega_{cv}
$$

---

### VIII. Numerical Example (pm = 50°)

Given:

$$
f_{sw} = 10\,\text{kHz}
$$

$$
L_f = 3.5\,\text{mH}, \quad R_f = 0.1\,\Omega
$$

$$
C_f = 50\,\mu\text{F}
$$

---

#### A. Current Loop

$$
\omega_{ci} = \frac{2\pi \cdot 10000}{4} = 15708
$$

$$
K_{pc} = 54.98
$$

$$
K_{ic} = 1570.8
$$

---

#### B. Voltage Loop

$$
\sin(50^\circ) = 0.766
$$

$$
\omega_{cv} = 2082
$$

$$
\omega_{gc} = 5719
$$

$$
K_{pv} = 0.286
$$

$$
K_{iv} = 595
$$

---

### IX. Final Gains

#### Current Loop

$$
K_{pc} \approx 55
$$

$$
K_{ic} \approx 1571
$$

---

#### Voltage Loop

$$
K_{pv} \approx 0.286
$$

$$
K_{iv} \approx 595
$$


### X. Summary of Design Equations

| Loop | Quantity | Expression |
|---|---|---|
| Current loop | Plant | $G_i(s)=\frac{1}{L_f s+R_f}$ |
| Current loop | Controller | $C_i(s)=K_{pc}+\frac{K_{ic}}{s}$ |
| Current loop | Zero cancellation | $\frac{K_{ic}}{K_{pc}}=\frac{R_f}{L_f}$ |
| Current loop | Bandwidth | $\omega_{ci}=\frac{2\pi f_{sw}}{4}$ |
| Current loop | Proportional gain | $K_{pc}=L_f\omega_{ci}$ |
| Current loop | Integral gain | $K_{ic}=R_f\omega_{ci}$ |
| Current loop | Closed-loop TF | $\frac{i(s)}{i^\ast(s)}=\frac{\omega_{ci}}{s+\omega_{ci}}$ |
| Voltage loop | Effective plant | $G_v(s)=\frac{\omega_{ci}}{(s+\omega_{ci})}\cdot\frac{1}{C_f s}$ |
| Voltage loop | Controller | $C_v(s)=K_{pv}+\frac{K_{iv}}{s}$ |
| Voltage loop | Zero frequency | $\omega_{cv}=\frac{K_{iv}}{K_{pv}}$ |
| Voltage loop | Symmetric choice | $\omega_{gc}=\sqrt{\omega_{cv}\omega_{ci}}$ |
| Voltage loop | Phase-margin relation | $\omega_{cv}=\omega_{ci}\frac{1-\sin(pm)}{1+\sin(pm)}$ |
| Voltage loop | Proportional gain | $K_{pv}=C_f\omega_{gc}$ |
| Voltage loop | Integral gain | $K_{iv}=K_{pv}\omega_{cv}$ |

---

### XI. Numerical Example Summary

For $f_{sw}=10\,\text{kHz}$, $L_f=3.5\,\text{mH}$, $R_f=0.1\,\Omega$, $C_f=50\,\mu\text{F}$, and $pm=50^\circ$:

| Quantity | Value |
|---|---:|
| $\omega_{ci}$ | $15708\ \text{rad/s}$ |
| $K_{pc}$ | $54.98$ |
| $K_{ic}$ | $1570.8$ |
| $\sin(50^\circ)$ | $0.766$ |
| $\omega_{cv}$ | $2082\ \text{rad/s}$ |
| $\omega_{gc}$ | $5719\ \text{rad/s}$ |
| $K_{pv}$ | $0.286$ |
| $K_{iv}$ | $595$ |
