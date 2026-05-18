# Ex: 04 - ASK & FSK
## AIM:
Write a simple Python program for the modulation and demmodulation of ASK and FSK.

## TOOLS REQUIRED:
Google colab

## PROGRAM:

#### ASK:
```import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Low-pass filter function
def lpf(x, fc, fs):
    b, a = butter(4, fc / (0.5 * fs), btype='low')
    return lfilter(b, a, x)

# Parameters
fs = 1000      # Sampling frequency
fc = 50        # Carrier frequency
br = 10        # Bit rate
T = 1          # Time duration

# Time axis
t = np.arange(0, T, 1/fs)

# Message signal (random bits)
bits = np.random.randint(0, 2, br)

# Convert bits to digital waveform
msg = np.repeat(bits, fs // br)

# Adjust length to match time axis
msg = msg[:len(t)]

# Carrier signal
carrier = np.sin(2 * np.pi * fc * t)

# ASK Modulation
ask = msg * carrier

# ASK Demodulation
demod = lpf(ask * carrier, fc, fs)

# Decode bits
decoded = (demod[::fs // br] > 0.25).astype(int)

# Plotting
plt.figure(figsize=(10, 9))

plt.suptitle(
    "NAME: YUVANESH S P\nREG NO: 212224060315",
    fontsize=12,
    fontweight='bold'
)

# Message Signal
plt.subplot(4, 1, 1)
plt.plot(t, msg)
plt.title("Message Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

# Carrier Signal
plt.subplot(4, 1, 2)
plt.plot(t, carrier)
plt.title("Carrier Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

# ASK Modulated Signal
plt.subplot(4, 1, 3)
plt.plot(t, ask)
plt.title("ASK Modulated Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

# Decoded Bits
plt.subplot(4, 1, 4)
plt.step(range(len(decoded)), decoded, where='mid')
plt.title("Decoded Bits")
plt.xlabel("Bit Index")
plt.ylabel("Bit Value")
plt.grid(True)

# Layout adjustment
plt.tight_layout(rect=[0, 0, 1, 0.93])

# Show plot
plt.show()
```


#### FSK:
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Low-pass filter function
def lpf(x, fc, fs):
    b, a = butter(4, fc / (0.5 * fs), btype='low')
    return lfilter(b, a, x)

# Parameters
fs = 1000      # Sampling frequency
f1 = 30        # Frequency for bit 0
f2 = 70        # Frequency for bit 1
br = 10        # Bit rate
T = 1          # Time duration

# Time axis
t = np.arange(0, T, 1/fs)

# Samples per bit
bd = fs // br

# Message signal (random bits)
bits = np.random.randint(0, 2, br)

# Convert bits to digital waveform
msg = np.repeat(bits, bd)

# Match length with time axis
msg = msg[:len(t)]

# Carrier signals
c1 = np.sin(2 * np.pi * f1 * t)
c2 = np.sin(2 * np.pi * f2 * t)

# FSK Modulation
fsk = np.zeros_like(t)

for i, b in enumerate(bits):
    start = i * bd
    end = (i + 1) * bd

    if b == 1:
        fsk[start:end] = np.sin(2 * np.pi * f2 * t[start:end])
    else:
        fsk[start:end] = np.sin(2 * np.pi * f1 * t[start:end])

# Demodulation using correlation
d1 = lpf(fsk * c1, f1, fs)
d2 = lpf(fsk * c2, f2, fs)

# Decode bits
dec = []

for i in range(br):
    start = i * bd
    end = (i + 1) * bd

    energy1 = np.sum(d1[start:end] ** 2)
    energy2 = np.sum(d2[start:end] ** 2)

    if energy2 > energy1:
        dec.append(1)
    else:
        dec.append(0)

# Reconstruct demodulated signal
demod = np.repeat(dec, bd)

# Plotting
plt.figure(figsize=(10, 10))

plt.suptitle(
    "NAME : YUVANESH S P\nREG NO : 212224060315",
    fontsize=12,
    fontweight='bold'
)

# Message Signal
plt.subplot(5, 1, 1)
plt.plot(t, msg)
plt.title("Message Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

# Carrier f1
plt.subplot(5, 1, 2)
plt.plot(t, c1)
plt.title("Carrier f1 (bit 0)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

# Carrier f2
plt.subplot(5, 1, 3)
plt.plot(t, c2)
plt.title("Carrier f2 (bit 1)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

# FSK Modulated Signal
plt.subplot(5, 1, 4)
plt.plot(t, fsk)
plt.title("FSK Modulated Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

# Demodulated Signal
plt.subplot(5, 1, 5)
plt.plot(t[:len(demod)], demod)
plt.title("Demodulated Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

# Layout adjustment
plt.tight_layout(rect=[0, 0, 1, 0.93])

# Show plot
plt.show()
```

## OUTPUT WAVEFORM:
#### ASK:
<img width="978" height="886" alt="image" src="https://github.com/user-attachments/assets/366d9fca-7cc7-4e50-b856-49ee45b522a0" />



#### FSK:
<img width="989" height="985" alt="image" src="https://github.com/user-attachments/assets/af92ca6c-0731-4b83-9481-0477ab7d59df" />



## RESULT:
Thus, the ASK and FSK were performed using Google Colab.
