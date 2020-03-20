# Hodgkin-Huxley model
4 dimensional model for neuronal modelling in Julia

# The model

The Hodgkin–Huxley model, or conductance-based model, is a mathematical model that describes how action potentials in neurons are initiated and propagated. It is a set of nonlinear differential equations that approximates the electrical characteristics of excitable cells such as neurons and cardiac myocytes. 

The typical Hodgkin–Huxley model treats each component of an excitable cell as an electrical element. The lipid bilayer is represented as a capacitance (![C_m](https://render.githubusercontent.com/render/math?math=C_m)
), voltage-gated ion channels are represented by electrical conductances (![g_n](https://render.githubusercontent.com/render/math?math=g_n), where n is the specific ion channel) that depend on both voltage and time. Leak channels are represented by linear conductances (![g_L](https://render.githubusercontent.com/render/math?math=g_L)
). The electrochemical gradients driving the flow of ions are represented by voltage sources (![E_n](https://render.githubusercontent.com/render/math?math=E_n)
) whose voltages are determined by the ratio of the intra and extracellular concentrations of the ionic species of interest. Finally, ion pumps are represented by current sources (![I_p](https://render.githubusercontent.com/render/math?math=I_p)
). The membrane potential is denoted by ![V_m](https://render.githubusercontent.com/render/math?math=V_m).

Mathematically, the current flowing through the lipid bilayer is written as

![{\displaystyle I_{c}=C_{m}{\frac {{\mathrm {d} }V_{m}}{{\mathrm {d} }t}}}](https://render.githubusercontent.com/render/math?math=%7B%5Cdisplaystyle%20I_%7Bc%7D%3DC_%7Bm%7D%7B%5Cfrac%20%7B%7B%5Cmathrm%20%7Bd%7D%20%7DV_%7Bm%7D%7D%7B%7B%5Cmathrm%20%7Bd%7D%20%7Dt%7D%7D%7D)

and the current through a given ion channel is the product

![{\displaystyle I_{i}={g_{i}}(V_{m}-V_{i})\;}](https://render.githubusercontent.com/render/math?math=%7B%5Cdisplaystyle%20I_%7Bi%7D%3D%7Bg_%7Bi%7D%7D(V_%7Bm%7D-V_%7Bi%7D)%5C%3B%7D)

where ![{\displaystyle V_{i}}](https://render.githubusercontent.com/render/math?math=%7B%5Cdisplaystyle%20V_%7Bi%7D%7D)
 is the reversal potential of the i-th ion channel. Thus, for a cell with sodium and potassium channels, the total current through the membrane is given by:

![{\displaystyle I=C_{m}{\frac {{\mathrm {d} }V_{m}}{{\mathrm {d} }t}}+g_{K}(V_{m}-V_{K})+g_{Na}(V_{m}-V_{Na})+g_{l}(V_{m}-V_{l})}](https://render.githubusercontent.com/render/math?math=%7B%5Cdisplaystyle%20I%3DC_%7Bm%7D%7B%5Cfrac%20%7B%7B%5Cmathrm%20%7Bd%7D%20%7DV_%7Bm%7D%7D%7B%7B%5Cmathrm%20%7Bd%7D%20%7Dt%7D%7D%2Bg_%7BK%7D(V_%7Bm%7D-V_%7BK%7D)%2Bg_%7BNa%7D(V_%7Bm%7D-V_%7BNa%7D)%2Bg_%7Bl%7D(V_%7Bm%7D-V_%7Bl%7D)%7D)

where ![I](https://render.githubusercontent.com/render/math?math=I) is the total membrane current per unit area, ![C_m](https://render.githubusercontent.com/render/math?math=C_m) is the membrane capacitance per unit area, ![g_K](https://render.githubusercontent.com/render/math?math=g_K) and ![g_{Na}](https://render.githubusercontent.com/render/math?math=g_%7BNa%7D) are the potassium and sodium conductances per unit area, respectively, ![V_K](https://render.githubusercontent.com/render/math?math=V_K) and ![V_{Na}](https://render.githubusercontent.com/render/math?math=V_%7BNa%7D) are the potassium and sodium reversal potentials, respectively, and ![g_l](https://render.githubusercontent.com/render/math?math=g_l) and ![V_l](https://render.githubusercontent.com/render/math?math=V_l) are the leak conductance per unit area and leak reversal potential, respectively. The time dependent elements of this equation are ![V_m](https://render.githubusercontent.com/render/math?math=V_m), ![g_{Na}](https://render.githubusercontent.com/render/math?math=g_%7BNa%7D), and ![g_K](https://render.githubusercontent.com/render/math?math=g_K), where the last two conductances depend explicitly on voltage as well.

It turns out that the channels are not always opened but the probability to find them opened depend on the ion concentrations, so the voltage. By denoting ![n](https://render.githubusercontent.com/render/math?math=n) the probability that the Na channel is opened, ![m](https://render.githubusercontent.com/render/math?math=m) the probability that the K channel is opend and ![h](https://render.githubusercontent.com/render/math?math=h) the probability that this last channel is actually activated (more information about this can be found in the references) we can construct a set of 4 diferential equations to model a neuron.

![\begin{equation*}\begin{array}{l}\displaystyle C_{m} \frac{d V}{d t}=\bar{G}_{N a} m^{3} h\left(E_{N a}-V\right)+\bar{G}_{K} n^{4}\left(E_{K}-V\right) +G_{m}\left(V_{r e s t}-V\right)+I \\\\  \displaystyle \frac{d x}{d t}=\alpha_{x}(V)(1-x)-\beta_{x}(V) x \end{array}\end{equation*}](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bequation*%7D%5Cbegin%7Barray%7D%7Bl%7D%5Cdisplaystyle%20C_%7Bm%7D%20%5Cfrac%7Bd%20V%7D%7Bd%20t%7D%3D%5Cbar%7BG%7D_%7BN%20a%7D%20m%5E%7B3%7D%20h%5Cleft(E_%7BN%20a%7D-V%5Cright)%2B%5Cbar%7BG%7D_%7BK%7D%20n%5E%7B4%7D%5Cleft(E_%7BK%7D-V%5Cright)%20%2BG_%7Bm%7D%5Cleft(V_%7Br%20e%20s%20t%7D-V%5Cright)%2BI%20%5C%5C%5C%5C%20%20%5Cdisplaystyle%20%5Cfrac%7Bd%20x%7D%7Bd%20t%7D%3D%5Calpha_%7Bx%7D(V)(1-x)-%5Cbeta_%7Bx%7D(V)%20x%20%5Cend%7Barray%7D%5Cend%7Bequation*%7D)

with ![x = n, m, h](https://render.githubusercontent.com/render/math?math=x%20%3D%20n%2C%20m%2C%20h) and ![\alpha, \ \beta](https://render.githubusercontent.com/render/math?math=%5Calpha%2C%20%5C%20%5Cbeta) are empirical equations, which can be found for instance in <sup>1</sup>.

# Some results

The results shown below are obtained with an integration time step of ![h_{\mathrm{int}}=10^{-3}](https://render.githubusercontent.com/render/math?math=h_%7B%5Cmathrm%7Bint%7D%7D%3D10%5E%7B-3%7D), a total time of ![t=200\mathrm{ms}](https://render.githubusercontent.com/render/math?math=t%3D200%5Cmathrm%7Bms%7D) and the initial values of the evolved magnitudes ![V_0=0](https://render.githubusercontent.com/render/math?math=V_0%3D0), ![n_0=m_0=h_0=0.5](https://render.githubusercontent.com/render/math?math=n_0%3Dm_0%3Dh_0%3D0.5). 

First results are shown for the evolution in time of the membrane potential for an intensity value of ![I=150\mathrm{pA}](https://render.githubusercontent.com/render/math?math=I%3D150%5Cmathrm%7BpA%7D).

![Membrane voltage as a function of time](Results/V_t_150.png)

As we can see with this input current value the stimulus is not enough to excite the neuron.

Now we increase the intensity up to ![I=170\mathrm{pA}](https://render.githubusercontent.com/render/math?math=I%3D170%5Cmathrm%7BpA%7D)

![Membrane voltage as a function of time](Results/V_t_170.png)

Now the neuron get excited for a short period of time but it relaxes finally to a steady state with any response.

Finally we increase the input current value to ![I=280\mathrm{pA}](https://render.githubusercontent.com/render/math?math=I%3D280%5Cmathrm%7BpA%7D)

![Membrane voltage as a function of time](Results/V_t_280.png)

As we can see in the figure, for this values of the input current the neuron is effectively excited and there are several spikes. In fact, the neuron would remain spiking if the simulation was continued, as for this values of the input current the dynamical system follows a limit cycle.

So, as we increase the intensity the number of spikes increases too. In fact the frequency of spikes increases almost linearly after a certain threshold. With the program in this repository this can be computed too.

![Frequency vs Intensity](Results/freq_150_350.png)

As we can see the pulsing intensity is ![\nu=0](https://render.githubusercontent.com/render/math?math=%5Cnu%3D0) until a threshold is reached and a big jump is performed to ![f\sim 50\mathrm{Hz}](https://render.githubusercontent.com/render/math?math=f%5Csim%2050%5Cmathrm%7BHz%7D). After that the frequency increases almost linearly. This kind of behaviour (this discontinuous jump, basically) is characteristic of a type II excitable system.
        
The statistical errors made in the measurements of the pulsating frequency is represented with a red shaded area. However, their values are so small that they can't be seen.

Now we can do the same calculation but starting from a high input current value:

![Frequency vs Intensity](Results/Freq_350_150.png)

By computing the spiking frequency starting from a high input current intensity value and decreasing it we observe that the threshold for the excitability of the neuron has shifted to ![I\sim177\mathrm{pA}](https://render.githubusercontent.com/render/math?math=I%5Csim177%5Cmathrm%7BpA%7D). By plotting both results simultaneously we see that the neurons perform a kind of hysteresis loop.

![Hysteresis](Results/Histeresis.png)

# References
1. [Wikipedia](https://en.wikipedia.org/wiki/Hodgkin%E2%80%93Huxley_model)
2. [W. Gerstner lectures](https://icwww.epfl.ch/~gerstner/VideoLecturesGerstner.html)
