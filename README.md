# EE 332: Digital Communications - Assignment 1
**Authors**: Akash Shaw (220102108), Ansul Jain (220102119)

---

## **Code Structure**
### **Question 1**: Sinusoidal Signal Analysis
- **File**: `Q1/main.grc`
- **Description**:
  - Generates a sine wave, plots time-domain and frequency-domain signals.
  - Sample rate is derived from the time-domain plot.

### **Question 2**: Digital Modulation (BPSK/QPSK/QAM)
- **Files**:
  - `Q2_BPSK.grc`, `Q2_QPSK.grc`, `Q2_16QAM.grc`, `Q2_64QAM.grc`
- **Description**:
  - Modulates random data using BPSK/QPSK/16-QAM/64-QAM.
  - Visualizes constellation plots with/without AWGN (variance = 0.01).

### **Question 3**: Pulse Shaping (NRZ/RZ/Manchester)
- **Provided Code**:
  - `NRZ.grc` (100 kbps QPSK with NRZ pulse shape).
- **Modifications for 300/500 kbps**:
  1. **Symbol Rate**:
     - For **300 kbps**: `symbol_rate = 300e3 / 2 = 150e3`.
     - For **500 kbps**: `symbol_rate = 500e3 / 2 = 250e3`.
  2. **Sample Rate**:
     - Ensure `samp_rate` is a multiple of `symbol_rate`.
       - **300 kbps**: Set `samp_rate = 3.15e6` (interpolation factor = 21).
       - **500 kbps**: Set `samp_rate = 3.25e6` (interpolation factor = 13).
  3. **Update in Code**:
     - Change `samp_rate` and `interpolation_factor` in the `.grc` and Python files.
     - Example for 300 kbps:
       ```python
       self.samp_rate = 3150000
       self.interpolation_factor = self.samp_rate // 150000  # = 21
       ```
  4. **Pulse Shape Taps**:
     - **RZ**: `np.concatenate([np.ones(N//2), np.zeros(N//2)])`.
     - **Manchester**: `np.concatenate([np.ones(N//2), -np.ones(N//2)])`.

---

## **Theoretical Explanations and Conclusions**

### **Question 1: Sinusoidal Signal Analysis**
**Theory**:
- A sinusoidal signal in the **time domain** is characterized by its amplitude, frequency, and phase. In the **frequency domain**, it appears as a delta function at the signal’s frequency.
- **Sample rate** determines the number of samples per second. According to the Nyquist theorem, the sample rate must be at least twice the signal frequency to avoid aliasing.

**Conclusion**:
- The sample rate can be calculated from the time-domain plot by measuring the time between consecutive peaks (`T`) and using `sample_rate = 1 / (T / N)`, where `N` is the number of samples per period.
- Higher sample rates improve signal resolution but increase computational complexity.

---

### **Question 2: Digital Modulation (BPSK/QPSK/QAM)**
**Theory**:
- **BPSK/QPSK**: Use phase shifts to encode bits (1 bit/symbol for BPSK, 2 bits/symbol for QPSK).
- **QAM**: Combines amplitude and phase modulation (e.g., 16-QAM encodes 4 bits/symbol).
- **AWGN**: Adds noise with a Gaussian distribution, degrading the signal-to-noise ratio (SNR).

**Conclusion**:
- **Constellation plots** show distinct clusters for BPSK/QPSK but spread under noise. Higher-order QAM (e.g., 64-QAM) is more noise-sensitive due to closer symbol spacing.
- Increasing noise variance (e.g., from 0.01 to 0.1) causes overlapping clusters, leading to higher bit error rates (BER).

---

### **Question 3: Pulse Shaping (NRZ/RZ/Manchester)**
**Theory**:
- **NRZ**: Full-width pulses; bandwidth-efficient but prone to baseline wander.
- **RZ**: Half-width pulses; reduces DC components but doubles bandwidth.
- **Manchester**: Mid-symbol transitions ensure synchronization and eliminate DC offset.
- **4-PAM/QPSK**: 4-PAM uses 4 amplitude levels (2 bits/symbol), while QPSK uses 4 phases (2 bits/symbol).

**Conclusion**:
- **Spectrum comparison**:
  - NRZ has a sinc-shaped spectrum with significant sidelobes.
  - RZ has wider bandwidth due to shorter pulses.
  - Manchester suppresses low-frequency components, ideal for AC-coupled channels.
- **Symbol rate** can be derived from spectral nulls (cyclostationary properties). For QPSK, `bitrate = 2 × symbol_rate`.

---

### **Question 4: QPSK with RC Pulse Shaping**
**Theory**:
- **Raised Cosine (RC)** pulses use excess bandwidth (10%–75%) to reduce inter-symbol interference (ISI).
- **Eye diagrams** assess signal quality: open eyes indicate minimal ISI.

**Conclusion**:
- **Spectrum**: Higher excess bandwidth increases spectral occupancy but improves ISI resilience.
- **Eye diagrams**: Wider eyes are observed with higher excess bandwidth (e.g., 75% vs. 10%).

---

### **Question 5: BPSK/16-QAM with RRC and Passband Conversion**
**Theory**:
- **Root Raised Cosine (RRC)** filters minimize ISI while maintaining matched filtering.
- **Carrier modulation** shifts the baseband signal to passband. Bandwidth depends on the symbol rate and excess bandwidth.

**Conclusion**:
- **BPSK** has a narrower bandwidth than **16-QAM** for the same bitrate.
- Adjusting the carrier frequency shifts the spectrum but does not alter the baseband bandwidth.

---

### Bit Rate Comparison Analysis

1. **Symbol Rate Calculations**
- For QPSK modulation (2 bits/symbol):
  - 100 kbps → 50 kHz symbol rate
  - 300 kbps → 150 kHz symbol rate
  - 500 kbps → 250 kHz symbol rate

2. **Bandwidth Requirements**
- Higher bit rates require more bandwidth:
  - 100 kbps: Baseline bandwidth
  - 300 kbps: ~3x bandwidth of 100 kbps
  - 500 kbps: ~5x bandwidth of 100 kbps

3. **Sampling Rate Considerations**
- Different sampling rates are needed:
  - 100 kbps: Standard sampling rate
  - 300 kbps: 3.15 MHz (interpolation factor = 21)
  - 500 kbps: 3.25 MHz (interpolation factor = 13)

4. **Impact on System Performance**

| Aspect | 100 kbps | 300 kbps | 500 kbps |
|--------|----------|----------|-----------|
| Bandwidth Efficiency | Most efficient | Moderate | Least efficient |
| ISI Susceptibility | Lower | Moderate | Higher |
| System Complexity | Lower | Moderate | Higher |
| Processing Requirements | Lower | Moderate | Higher |

5. **Practical Implications**
- Higher bit rates:
  - Require more sophisticated hardware
  - More susceptible to noise and interference
  - Need better synchronization
  - Have stricter timing requirements

6. **Trade-offs**
- Lower bit rates (100 kbps):
  - More robust against noise
  - Easier to implement
  - Lower hardware requirements
- Higher bit rates (500 kbps):
  - Better throughput
  - More challenging implementation
  - Higher hardware requirements

---

## **Dependencies**
- GNU Radio Companion (v3.10+)
- Python 3.8+
- NumPy, PyQt5

---

## **How to Run**
1. Open `.grc` files in GNU Radio Companion.
2. Click the "Generate" button to create Python scripts.
3. Execute the scripts:
   ```bash
   python3 <filename>.py
    ```
