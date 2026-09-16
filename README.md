# Hodgkin–Huxley Model of DBS Electrode Aging

> Simulating how deep brain stimulation electrode degradation affects neural firing over time.

## Motivation

Deep brain stimulation electrodes lose effectiveness over months to years as tissue encapsulation and corrosion raise impedance, so less of the programmed current actually reaches the target neurons. Clinically, this raises an obvious question: does a patient's response fade out gradually, giving doctors time to notice and reprogram the device, or does it fail without warning? I built this model to answer that using a single Hodgkin-Huxley neuron as a stand-in for the stimulated tissue.

## What's in the Notebook

* **Core Model:** A standard 4-variable HH neuron ($V, m, h, n$), integrated with `scipy.solve_ivp`.
* **Threshold Analysis:** An F-I (frequency-current) sweep to find the firing threshold.
* **Degradation Profiles:** Three degradation curves for effective stimulus current over a 24-month timeline—linear, exponential, and sigmoidal—designed to represent different corrosion patterns.
* **Longitudinal Output:** Simulated spike output across the entire 24-month window for each degradation curve.
* **Voltage Traces:** Snapshots of the firing dynamics at four specific points along the sigmoidal decline (100%, 70%, 45%, and 30% of nominal current) to visualize what firing actually looks like at each stage.
* **Predictive Testing:** A noise-injection test near the threshold to check whether spike reliability drops off before firing stops completely.
## Results and interpretation
The F-I curve gave a firing threshold at I ≈ 2.37 µA/cm².Below it, the neuron doesn't fire.
I connected that threshold to the degradation curves. Exponential decay burns through most of its current loss early, so it hit the failure threshold by month 13. Linear decay fails later, around month 19, with a somewhat visible decline in spike count in the months leading up to it. 

But in the sigmoidal model, current output barely moves for the first year(staying above 9.9 µA/cm² through month 8), then collapses over about four months. Spike count follows the same shape: 13-14 spikes all the way to month 15, then zero by month 19.

The obvious limitation is that this is one HH neuron with squid-axon parameters and idealized degradation curves, not validated against real impedance measurements or patient data.
