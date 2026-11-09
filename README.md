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

Part B – Reduce a 50 Hz Hum in Audio (MATLAB)

<img width="1920" height="1080" alt="Screenshot (161)" src="https://github.com/user-attachments/assets/e51ddf3b-af71-410f-93c0-37aa43dcfa35" />
<img width="1920" height="1080" alt="Screenshot (160)" src="https://github.com/user-attachments/assets/6cce3033-32c3-4452-a062-ec48473c11d0" />
<img width="1920" height="1080" alt="Screenshot (159)" src="https://github.com/user-attachments/assets/4cf0e0e0-4843-4089-b99c-52820c1e2a1a" />
<img width="1920" height="1080" alt="Screenshot (158)" src="https://github.com/user-attachments/assets/d22a88c7-d298-4306-a4cd-3f9bf4107d59" />
<img width="1920" height="1080" alt="Screenshot (157)" src="https://github.com/user-attachments/assets/96caf71f-1f91-4984-b4a7-2fa75e4d1672" />
2. Important measurements and expected observations
Narrow filter: 49–51 Hz
This filter removes frequencies close to 50 Hz while protecting most neighbouring audio frequencies.
Wide filter: 45–55 Hz
This filter removes a wider range of frequencies around 50 Hz. It may be more effective if the electrical hum changes slightly in frequency, but it can also remove useful low-frequency audio information.

Important correction for reliable measurements: The FFT bin nearest 50 Hz is an estimate, not necessarily the exact amplitude of the hum. Since you know the artificial hum, you can isolate its filtered contribution by filtering the clean audio separately and subtracting it from the filtered noisy audio. This gives a more meaningful measurement.
Why this version is better:
- It uses the same playback gain for all recordings with sound, rather than soundsc, which automatically rescales each recording.
- It isolates the added hum before measuring its amplitude.
- It compares narrow and wide filtering against the original clean audio using RMSE.
- It calculates attenuation in decibels.
  Question 1. Why is a 20 Hz low-pass reasonable for Part A, and what assumption does it make?
A 20 Hz low-pass filter is reasonable because the wanted sensor signal contains 5 Hz and 12 Hz components. The unwanted interference occurs at 50 Hz and 137 Hz, which are above the selected cutoff frequency.

The filter reduces high-frequency interference while allowing the wanted measurement components to pass through.

The main assumption is that the important information in the sensor signal is below 20 Hz. If useful information exists above 20 Hz, the filter could remove part of that information.
Question 2. Which filter gives the better result for the requirements you chose?
For Part A, the FIR and Butterworth IIR filters can be compared using two main types of evidence:
1. RMSE: The filter with the lower RMSE produces an output closer to the original clean signal.
2. Frequency spectrum: The spectrum shows how effectively each filter reduces the unwanted 50 Hz and 137 Hz components while preserving the 5 Hz and 12 Hz components.
The FIR filter uses 61 coefficients, while the IIR filter uses only 5 numerator and 5 denominator coefficients.

For Part B, the narrow and wide band-stop filters should be compared using hum attenuation and RMSE.

The narrow filter is preferable when the interference remains close to 50 Hz and preserving nearby audio frequencies is important. The wide filter may be preferable when the hum frequency varies.

The final choice should be based on the measured results rather than assuming that one filter is always better.
Question 3. How would your implementation change if the sensor had to be filtered live with low latency?
If the sensor signal had to be filtered live, I would use real-time filtering instead of filtfilt.

The filtfilt function processes the signal forward and backward, meaning it requires the complete signal and is not suitable for normal real-time streaming.

For live filtering, I would:
- Use filter() instead of filtfilt().
- Process incoming sensor samples continuously.
- Maintain filter states between processing blocks.
- Choose a low-order filter to reduce computational requirements.
- Measure processing time and latency.
- Consider an IIR filter when low computational cost is important.
This would allow the sensor system to process measurements continuously with low latency.
Question 4. Why is a narrow band-stop appropriate for a steady 50 Hz hum? What would you do if the interference frequency drifted?
A narrow band-stop filter is appropriate because electrical hum is usually concentrated around 50 Hz.

A narrow stop band between 49 Hz and 51 Hz removes the unwanted frequency while preserving most nearby audio frequencies.

However, if the interference frequency changes, a narrow filter might not remove all the hum.

For example, if the hum changes between 48 Hz and 52 Hz, a wider stop band could be more effective.

Another solution would be to use an adaptive notch filter that automatically tracks changes in the interference frequency.
Question 5. Describe one limitation of your experiments and one improvement you would make in a real measurement system.
One limitation of this experiment is that the artificial interference is generated using a fixed 50 Hz sine wave. Real electrical interference can vary in frequency and amplitude, and real sensor signals may contain additional noise sources.

Another limitation is that filtfilt is an offline filtering method and cannot directly represent a continuously operating real-time system.

An improvement would be to implement an adaptive filtering system using a microcontroller such as an ESP32. The system could continuously collect sensor measurements, detect unwanted interference and adjust its filtering parameters.


