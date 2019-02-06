# tinyCurrent Test Procedure

## Resistive Load

* Multimeter connected to the Device under Test (DUT) output
* 5 VCC Load power supply
* Device turned ON
* Test the following current ranges and respective loads:

|Current Range  |Load (precise value) [Ohms]	|Expected Output [mV]	|Min [mV]		|Max [mV]		|Accuracy	|
|--:		|--:				|--:			|--:			|--:			|--:		|
|1mV/nA   	|50M (52.7)			|95	   		|90.2			|99.7			|0.05%		|
|1mV/uA   	|47K (46.80)   			|106.83   		|101.4			|112.2			|0.05%		|
|1mV/mA   	|47R (47.20)	   		|105.93			|95.3			|116.56			|0.1%		|


## Offset Voltage

* Power switch to the SHORT position.
* The current range doesn't matter as it is shorted.
* Measure the output voltage with a multimeter (DMM7510) and avoid touching the setup.

| Expected Outout Value	|
|--:			|
| < 30 µV		|

In comparision some uCurrent Gold with TI opamp can haveup to 200 µV as can be seen [here](https://youtu.be/1VlKoR0ldIE?t=1709).

## Offset Voltage with a capacitive Load

Capacitive loads on the output terminals might cause instability in the virtual ground OpAmp and deteriorate the offset voltage performance. The DUT has to be tested the following loads and its expected to not show any changes on its typical offset voltage.

| Capacitive Loads	| 
|--:			|
| 680pF			|
| 2.04nF (3x680pF)	|
| 3.4nf (5x680pF)	|
| 100nF			|
| 10uF			|
| 100uF			|


## AC Performance and Noise

### Bandwidth

The bandwidth can be measured by finding the -3dB cutoff frequency. The tinyCurrent has a low-pass filter response typical from OpAmps, so the -3dB cutoff frequency is where the amplitude of the output signal is 0.707 of the DC output voltage. Assuming that the amplitude at 8 Hz is approximately the same at 0 Hz, the cuttoff frequency can be found using the VSG and the DSO using the following procedure:

* Using a sine-sweep wave of -15dBm from 8 KHz to 800KHz. Set the Linear steps and the Dwell Time, so the signal goes slow enough for you to find the cuttoff frequency on the screen, The recommendation is 500 Hzfor the Linear steps and 10 ms for Dwell Time. 
* a 47 Ohms load and 1mV/uA (10 Ohms) current range. Remember that it must be in series configuration with the tinyCurrent! 
* Measure the output Vpp or amplitude at 8KHz. Use this value to find the amplitude at the cutoff frequency (0.707\*Amplitude) and set the horizontal cursors. Another way is just use the Vpp measurementi functionality.
* Now start the frequency sweep in the VSO and stop it when you find the cutoff frequency,
* For better precision, the frequency sweep can be done manually. 

### THD

THD can be measured by measuring the harmonic components of the original signal. These amplitudes can be measured by applying a FFT to the output signal.
Bear in mind that THD varies with frequency and amplitude. The recomendation is to use a fixed amplitude in the working range (2 Vpp in the output), and measure at some interesting points inside the bandwidth. For example: of 8kHz, 80kHz, and 160kHz.
The method to measure THD in a particular frequency is described in the following steps:

Using a fixed amplitude sine wave, and measuring the FFT at 8 Khz, 80 Khz, 160 Khz, 300 Khz , 450 Khz, 1MHz
* Vector signal generator sine output at -12 dBm, the important thing here is to not saturate the opamps. 
* 46R resistor in series with the tinyCurrent input, current range to 1mV/1uA; 
* DSO in high resolution signal acquisition, FFT using a Blackman window and average mode. 
* For each frequency of interest, the fundamental and its harmonics amplitudes have be written on a table like the following:

| Freq (kHz)	| Harmonic #	| Pout (dBm)	| Vout (V)	|
|--:		|--:		|--:		|--:		|
|		|1		|		|		|
|		|2		|		|		|
|		|3		|		|		|
|		|4		|		|		|
|		|5		|		|		|	
|		|6		|		|		|
|		|7		|		|		|

* Vout is calculated from the dBm values. After the conversion, the THD is ready to be calculated by the formula:

![](https://wikimedia.org/api/rest_v1/media/math/render/svg/4f9de3b50d754605a2f23e1a92f7dd682eb654a0 "THD_f")

Reference: 
* Method 2 at [microsemi AN30](https://www.microsemi.com/document-portal/doc_view/134813-an30-basic-total-harmonic-distortion-thd-measurement)
* Available in [wikipedia](https://en.wikipedia.org/wiki/Total_harmonic_distortion).  

### Noise Floor

Procedure:

* FFT rectangular window and measurements in dBV.
* The noise floor is measured after the 1/f noise frequency range.
* This measurement is subjected to the DSO noise, so first we measure with the tinyCurrent turned off, and then in the SHORT position
