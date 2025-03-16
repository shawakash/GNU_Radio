# Digital Communications Lab Assignment

## Authors
- Akash Shaw (220102108)
- Ansul Jain (220102119)

## Project Structure

### 1. Signal Analysis (`Q1/`)
- `main.grc`: Sinusoidal signal generation and analysis
- Sample rate: 32kHz
- Signal frequency: 1kHz

### 2. Digital Modulation (`Q2/`)
- `Q2_BPSK.grc`: BPSK modulation
- `Q2_QPSK.grc`: QPSK modulation
- `Q2_16QAM.grc`: 16-QAM modulation
- `Q2_64QAM.grc`: 64-QAM modulation
- AWGN variance: 0.01

### 3. Pulse Shaping (`Q3/`)
Three implementations for each bit rate (100/300/500 kbps):
- `NRZ/`: Non-Return-to-Zero implementation
- `RZ/`: Return-to-Zero implementation
- `Manchester/`: Manchester coding implementation

#### Bit Rate Modifications
For different bit rates:
```python
# 300 kbps
samp_rate = 3.15e6
interpolation_factor = samp_rate // 150000

# 500 kbps
samp_rate = 3.25e6
interpolation_factor = samp_rate // 250000
```

### 4. RC Pulse Shaping (`Q4/`)
- `RC.grc`: QPSK with RC pulse shaping
- Excess bandwidth variations: 10%, 25%, 50%, 75%

### 5. Passband Conversion (`Q5/`)
- `RRC_Pass.grc`: RRC pulse shaping with passband conversion
- Bit rate: 200 kbps
- Excess bandwidth: 35%
- Adjustable carrier frequency

## Implementation Details

### Sample Rate Selection
- 100 kbps: 3.2 MHz (default)
- 300 kbps: 3.15 MHz
- 500 kbps: 3.25 MHz

### Interpolation Factors
```python
interpolation_factor = samp_rate // symbol_rate
# where symbol_rate = bit_rate / bits_per_symbol
```

### Pulse Shaping Filters
```python
# NRZ
taps = np.ones(N)

# RZ
taps = np.concatenate([np.ones(N//2), np.zeros(N//2)])

# Manchester
taps = np.concatenate([np.ones(N//2), -np.ones(N//2)])
```

## Dependencies
- GNU Radio 3.10+
- Python 3.8+
- NumPy
- SciPy

## Running the Project
1. Open `.grc` files in GNU Radio Companion
2. Generate Python code
3. Run with:
   ```bash
   python3 generated_file.py
   ```

## Results and Analysis
Detailed analysis available in the accompanying report, including:
- Constellation diagrams
- Spectrum analysis
- Eye diagrams
- BER analysis
