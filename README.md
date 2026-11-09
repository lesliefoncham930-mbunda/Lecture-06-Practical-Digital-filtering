<img width="1920" height="1080" alt="Screenshot (156)" src="https://github.com/user-attachments/assets/cbe11462-6127-40a8-a1d6-9643afaa054d" />
<img width="1920" height="1080" alt="Screenshot (151)" src="https://github.com/user-attachments/assets/29ec12a5-4a52-456e-bb8c-f03dbd0b7236" />
<img width="1920" height="1080" alt="Screenshot (150)" src="https://github.com/user-attachments/assets/a636702d-8453-4501-8c0e-0784e96be115" />
<img width="1920" height="1080" alt="Screenshot (149)" src="https://github.com/user-attachments/assets/be6615f8-6fce-44bd-a57f-a41d15e239cc" />
2. Explanation of the expected results
Task 1: Signal generation
The clean signal contains two wanted frequencies:
- 5 Hz: Main measurement component.
- 12 Hz: Smaller measurement component.
The unwanted components are:
- 50 Hz interference.
- 137 Hz interference.
- Random Gaussian noise.
The purpose of filtering is to preserve the wanted 5 Hz and 12 Hz components while reducing unwanted frequencies.
Task 2: Filter design comparison
Feature	FIR filter	Butterworth IIR
Filter order	60	4
Coefficients	61	5 numerator, 5 denominator
Stability	Inherently stable	Stable for this design
Transition	Generally wider	Generally sharper
Phase	Linear phase in normal FIR operation	Nonlinear phase
Filtering method	filtfilt	filtfilt

Task 3: Expected frequency response
The filters have a 20 Hz cutoff frequency.
- 5 Hz: Should pass through both filters with little attenuation.
- 12 Hz: Should pass through, although some attenuation may occur because it is closer to the cutoff.
- 50 Hz: Should be strongly attenuated.
- 137 Hz: Should also be strongly attenuated.







blog.foerstergroup.com


Task 4: RMSE calculation
RMSE means Root Mean Square Error. It measures the average size of the difference between the filtered signal and the known clean signal.

The formula is:
\[
RMSE=\sqrt{\frac{1}{N}\sum_{n=1}^{N}(y[n]-clean[n])^2}
\]

Interpretation:
- Lower RMSE means the filtered signal is closer to the clean signal.
- Higher RMSE means there is a larger difference.
- The noisy input RMSE provides a baseline.
- Comparing FIR and IIR RMSE tells us which filter performs better for this particular signal.
Important: RMSE cannot tell us which frequencies were removed or whether the filter is suitable for every application. It also combines the effects of residual noise and unwanted attenuation of the wanted signal.
Task 5: Comparison and discussion
FIR filter
- Uses 61 coefficients.
- Requires more calculations than the 4th-order IIR filter.
- Has a relatively gradual transition with this design.
- Can provide linear phase before filtering with filtfilt.
- Useful when predictable phase characteristics and stability are important.
Butterworth IIR filter
- Uses 5 numerator and 5 denominator coefficients.
- Requires fewer coefficients.
- Provides a smooth, maximally flat passband.
- Generally achieves a sharper transition with a much lower order.
- Useful when computational efficiency and lower memory requirements are important.
Edge effects: Both filters use filtfilt, which processes the signal forward and backward. This removes phase delay but can introduce edge distortions because the algorithm uses signal padding near the beginning and end.

3. Conclusion for your assignment
You can use this conclusion in your report:

The experiment compared two low-pass filters, a 60th-order FIR filter and a 4th-order Butterworth IIR filter, using a noisy sensor signal sampled at 1000 Hz. Both filters had a cutoff frequency of 20 Hz and were applied using the filtfilt function to avoid phase delay.

The results show that low-pass filtering reduces unwanted high-frequency interference, particularly at 50 Hz and 137 Hz, while preserving the main measurement components at 5 Hz and 12 Hz. However, some attenuation of the 12 Hz component may occur because it is relatively close to the cutoff frequency.

The RMSE values help determine which filter produces an output closer to the original clean signal. The FIR filter uses more coefficients, while the Butterworth IIR filter achieves filtering with a much lower order.

In conclusion, both filters are useful for sensor signal processing. The FIR filter is a good choice when stability and predictable phase characteristics are important, while the IIR filter is preferable when computational efficiency and fewer coefficients are required.
