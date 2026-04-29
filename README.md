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
<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/9db104f7-1176-4481-b530-5812deeca56d" />

<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/5798c565-1107-4e10-8987-754fd909c198" />

<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/8c5fd217-11e1-4478-91f5-6f215bae795d" />

# Natural sampling
<img width="1390" height="989" alt="dc natural" src="https://github.com/user-attachments/assets/2f2c2e99-46c2-4e89-89d8-9bda85289aaa" />

# Flat-Top Sampling
<img width="1398" height="990" alt="image" src="https://github.com/user-attachments/assets/41ab1827-30ee-410b-8798-53e0fdb5e05e" />

# Results

Impulse sampling gives perfect reconstruction, while natural and flat-top sampling allow approximate reconstruction, with flat-top introducing slight amplitude distortion.


