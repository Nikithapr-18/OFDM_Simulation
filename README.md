# OFDM System Simulation, Performance Analysis Under Realistic Channel Impairments

A complete OFDM communication link built from first principles in NumPy/SciPy (no communications toolbox), used to validate core theory and quantify the cost of realistic impairments and PAPR-reduction trade-offs.

## Method

**Link chain (`OFDM_Simulation.ipynb`):**
1. Gray-coded bit mapping (BPSK / QPSK / 16-QAM / 64-QAM), normalized to unit symbol energy
2. IFFT + cyclic-prefix OFDM transmitter, CP-removal + FFT receiver, per-subcarrier equalization
3. Channel models: AWGN, frequency-selective Rayleigh/Rician multipath (tapped-delay-line), carrier-frequency offset (CFO)
4. Pilot-based channel estimation: Least Squares (LS) and a delay-spread-aware MMSE-style estimator
5. PAPR analysis (CCDF) with three reduction techniques: Clipping, Selective Mapping (SLM), Partial Transmit Sequences (PTS)

**Validation approach:**
- Every block checked individually (noiseless loopback, closed-form theoretical bounds) before being chained into the full link
- All results are Monte Carlo averages over independent bit/channel/noise realizations

## Key Results
- Noiseless loopback reconstruction error ≈5×10⁻¹⁶ — OFDM core (IFFT/FFT/CP) is exact
- AWGN BER matches closed-form theory across all four modulation orders (BPSK/QPSK best → 64-QAM worst)
- On a 6-tap Rayleigh channel: Perfect CSI > MMSE ≈ close behind > LS, with the LS gap widest at low SNR
- CFO: BER rises ~2 orders of magnitude between ε=0.02 and ε=0.05; by ε=0.3 BER (~0.48) approaches random guessing
- PAPR reduction (mean PAPR, from 6.69 dB baseline): Clipping → 5.95 dB, SLM (U=8) → 5.42 dB, PTS (M=4) → 4.91 dB — confirming PTS > SLM > Clipping
- SLM and PTS are distortion-free (zero BER cost, only side-information overhead); clipping's similar PAPR gain comes with a growing BER floor

**Conclusion:** the simulated link is theory-consistent end-to-end. MMSE estimation recovers most of the SNR penalty LS pays relative to perfect CSI; CFO degrades BER sharply and continuously; and PAPR reduction techniques face a real trade-off between complexity/overhead (SLM, PTS) and signal distortion (clipping).

## Requirements

Software:
- Python: numpy, scipy, matplotlib
