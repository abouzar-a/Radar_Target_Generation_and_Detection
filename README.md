# Radar_Target_Generation_and_Detection
Part of Udacity sensor fusion nanodegree  

# Radar Target Generation And Detection Project

[//]: # (Image References)

[image1]: ./images/FFT2.jpg "2D FFT"
[image2]: ./images/2D_CFAR.jpg "2D CFAR"
[image3]: ./images/FFT1.jpg "FFT"
[image4]: ./images/project_layout.png "Project Layout"
[image5]: ./images/radar.png "Radar"
[image6]: ./images/tgnd.png "Target Generation And Detection"
[image7]: ./images/radar_spec.png "Radar Specs"

![alt text][image5]

This project develops a Radar target generation and detection system as part of the Udacity Sensor Fusion Nanodegree program. The following text will cover each aspect outlined in the [project rubric].(https://review.udacity.com/#!/rubrics/2548/view). 

![alt text][image4]

## FMCW Waveform Design
In this project, an FMCW (Frequency-Modulated Continuous Wave) Radar has been modeled. The FMCW signal is varied in frequency over time, with the signal increasing and decreasing in a controlled manner. Based on the radar specifications provided in the image below, key FMCW wave characteristics, such as Chirp Time, Bandwidth, and Slope, can be calculated. Chirp Time is defined as the duration during which one upward or downward frequency sweep is transmitted by the radar. Bandwidth is represented by the range of frequencies covered by the signal, and Slope is calculated as the ratio of Bandwidth to Chirp Time.

![alt text][image7]

## Target Generation and Simulation
The target starts at a range of 80 meters with a velocity of -70 meters per second, assuming a constant velocity throughout the simulation. To model the interaction between the radar and the target, the simulation iterates through an array of evenly spaced timestamps, updating the target's range at each interval. The transmitted and received signals are described by the equations provided in the image below. The received signal is a delayed version of the transmitted one, with the delay representing the time taken for the signal to travel to the target and back. By combining the transmitted and received signals, the beat signal is produced, containing information on both range and velocity (Doppler shift). 

![alt text][image6]

## Range first FFT
Applying a 1D FFT to the beat signal along the range axis allows us to extract the target's original position information.

![alt text][image3]

Output of 2D-FFT

![alt text][image1]
## 2D CFAR
To achieve accurate target identification, the 2D CFAR technique is employed to establish a noise suppression threshold. This process begins by configuring the number of training and guard cells in both the range and Doppler dimensions, along with determining an offset value to fine-tune the Signal-to-Noise Ratio (SNR). The offset, set to 1.2, adjusts the sensitivity of the detection process. The selected parameters are:

Training Cells:
Range (Tr): 10
Doppler (Td): 8
Guard Cells:
Range (Gr): 4
Doppler (Gd): 4

With the hyperparameters configured, the CFAR sliding window technique is applied to the matrix, incorporating a margin to effectively manage edge cells. This margin ensures that edge cells are designated as training cells rather than Cells Under Test (CUT), allowing comprehensive noise suppression throughout the matrix. In each iteration, the signal levels within the training cells are summed and converted from decibels to a linear scale using the "db2pow" function. The average of these linear values is calculated, then converted back to decibels. An offset is added to this average to set the detection threshold. The signal in the CUT is then compared to this threshold: a value of 1 is assigned if the signal exceeds the threshold, otherwise, a 0 is assigned. 

![alt text][image2]


