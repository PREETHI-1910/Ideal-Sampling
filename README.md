# Ideal, Natural, & Flat-top -Sampling
# Aim
Write a simple Python program for the construction and reconstruction of ideal, natural, and flattop sampling.
# Tools required

 Google Colab
 
# Program
# Impulse Sampling 
```
# Impulse Sampling 

import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import resample

fs = 100
t = np.arange(0, 1, 1/fs)
f = 5

x = np.sin(2*np.pi*f*t)

ts = np.arange(0, 1, 1/fs)
xs = np.sin(2*np.pi*f*ts)

xr = resample(xs, len(t))

# SAME SIZE (like Natural Sampling)
plt.figure(figsize=(14, 10))

# Original Signal
plt.subplot(3,1,1)
plt.plot(t, x)
plt.title("Original Signal")
plt.grid()

# Sampled Signal
plt.subplot(3,1,2)
plt.stem(ts, xs)
plt.title("Impulse Sampled Signal")
plt.grid()

# Reconstructed Signal
plt.subplot(3,1,3)
plt.plot(t, xr, 'r--')
plt.title("Reconstructed Signal")
plt.grid()

plt.tight_layout()
plt.show()

```

# Natural sampling 
```
# 2. Natural Sampling

import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs = 1000
t = np.arange(0, 1, 1/fs)
f = 5

x = np.sin(2*np.pi*f*t)

pulse_rate = 50
step = int(fs/pulse_rate)

sampled = np.zeros_like(t)

for i in range(0, len(t), step):
    sampled[i] = x[i]

def lp(sig):
    b, a = butter(5, (2*f)/(0.5*fs), btype='low')
    return lfilter(b, a, sig)

xr = lp(sampled)

plt.subplot(3,1,1)
plt.plot(t, x)
plt.title("Original")

plt.subplot(3,1,2)
plt.stem(t, sampled)
plt.title("Natural Sampling")

plt.subplot(3,1,3)
plt.plot(t, xr)
plt.title("Reconstructed")

plt.tight_layout()
plt.show()

```

# Flat-Top Sampling
```
# 3. Flat-top Sampling

import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs = 1000
t = np.arange(0, 1, 1/fs)
f = 5

x = np.sin(2*np.pi*f*t)

pulse_rate = 50
step = int(fs/pulse_rate)

sampled = np.zeros_like(t)

for i in range(0, len(t), step):
    sampled[i:i+10] = x[i]   # flat-top

def lp(sig):
    b, a = butter(5, (2*f)/(0.5*fs), btype='low')
    return lfilter(b, a, sig)

xr = lp(sampled)

plt.subplot(3,1,1)
plt.plot(t, x)
plt.title("Original")

plt.subplot(3,1,2)
plt.plot(t, sampled)
plt.title("Flat-top Sampling")

plt.subplot(3,1,3)
plt.plot(t, xr)
plt.title("Reconstructed")

plt.tight_layout()
plt.show()

```

# Output Waveform

# Impulse Sampling 
<img width="1398" height="989" alt="image" src="https://github.com/user-attachments/assets/079c6e82-36b7-4637-9604-e62aba5e0a7a" />

# Natural sampling
<img width="630" height="470" alt="image" src="https://github.com/user-attachments/assets/61bc16b7-348f-48ff-ac8e-32091eab00ca" />

# Flat-Top Sampling
<img width="630" height="470" alt="image" src="https://github.com/user-attachments/assets/d108f5b2-fb32-45ef-bdde-2ba0ddd56952" />

# Results

Impulse sampling gives perfect reconstruction, while natural and flat-top sampling allow approximate reconstruction, with flat-top introducing slight amplitude distortion.


